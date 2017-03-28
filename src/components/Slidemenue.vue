<template xmlns:v-bind="http://www.w3.org/1999/xhtml">
  <div class="slide-menue">
    <div class="menue" ref="slideMenu" :class="{menuetransition:isAnimate}" :style="{width:width+'px',transform: 'translate3d('+width*-1+'px'+',0,0)'}">
      <slot></slot>
    </div>
    <div class="mask" ref="mask" @click="closeMask($event)">
    </div>
  </div>

</template>

<script>
  //poX:侧滑菜单的位置
  export default {
    data (){
      return {
        isAnimate: false, //控制过度的作用时机，不可设置
        beforemoveX: 0, //上一次移动的距离
        currmoveX: 0,  //本次移动新增的距离
        startX: 0,     //按下时的横向位置
        distanceX: 0,  //最终的移动距离
        isMsk: false,  //控制遮罩层的开关
      }
    },
    props:{
      width:Number
    },
    watch: {
      currmoveX(val){
        //最终的移动距离等于上次移动的距离加上本次移动新增的距离
        this.distanceX = val + this.beforemoveX
        if (this.distanceX > this.width) {
          this.distanceX = this.width;
        }
      },
      distanceX(val){
        var m = -this.width + val
        this.$refs.slideMenu.style.transform = 'translate3d(' + m + 'px,0,0)'
      },
      isMsk(val){
        if (val){
          this.$refs.mask.style.opacity=1
        }else {
          this.$refs.mask.style.opacity=0
          this.$refs.mask.style.zIndex=-20
        }
      }
    },
    mounted(){
      var _this = this
      document.addEventListener('touchstart', function (e) {
        _this.isAnimate = false
        _this.startX = e.changedTouches[0].pageX
      });
      document.addEventListener('touchmove', function (e) {
          _this.currmoveX= e.changedTouches[0].pageX - _this.startX
      })
      document.addEventListener('touchend', function (e) {
             //抬起手指记录本次移动的距离作为下次的beforemoveX
        _this.beforemoveX = _this.distanceX
        _this.isAnimate = true
        if (_this.currmoveX > 30 ||-30< _this.currmoveX < 0) {
          _this.distanceX = _this.beforemoveX = _this.width;
          _this.isMsk = true
        }else if (
          _this.currmoveX < -30 ||0< _this.currmoveX < 30) {
          _this.distanceX = _this.beforemoveX = 0
          _this.isMsk = false
        }
      })
    },

    methods: {
      closeMask(e){
        if (e.target.className == "mask") {
          this.$refs.mask.style.opacity=0
          this.distanceX = this.beforemoveX = 0
        }
      },
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .menue {
    height: 100%;
    background-color: #eee;
    position: absolute;
    top: 0;
    z-index: 10;
  }

  .mask {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 9;
    opacity: 0;
    transition: 0.3s;
  }

  .menuetransition {
    transition: 0.3s;
  }
</style>
