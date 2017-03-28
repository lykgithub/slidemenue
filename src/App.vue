<template>
  <div id="app">
    <slide-menue :isAnimate="isAnimate" :width="width" :posX=distanceX :showMsk='isMsk' @close="closeMenue">
      <div class="content">
        <div class="user-header">
          <img src="./components/images/image.jpg"/>
        </div>
        <div class="menue-content">
          <ul>
            <li>列表1</li>
            <li>列表2</li>
            <li>列表3</li>
          </ul>
        </div>
      </div>
    </slide-menue>
  </div>
</template>

<script>
  import slideMenue from './components/Slidemenue'
  export default {
    name: 'app',
    components: {
      slideMenue
    },
    data (){
      return {
        isAnimate: false, //控制过度的作用时机，不可设置
        beforemoveX: 0, //上一次移动的距离
        currmoveX: 0,  //本次移动新增的距离
        startX: 0,     //按下时的横向位置
        distanceX: 0,  //最终的移动距离
        isMsk: true,  //控制遮罩层的开关
        width: 300
      }
    },
    watch: {
      currmoveX(val){
        //最终的移动距离等于上次移动的距离加上本次移动新增的距离
        this.distanceX = val + this.beforemoveX
        if (this.distanceX > this.width) {
          this.distanceX = this.width;
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
        e.preventDefault();
        _this.currmoveX = e.changedTouches[0].pageX - _this.startX;
      })
      document.addEventListener('touchend', function (e) {
        _this.beforemoveX = _this.distanceX;  //抬起手指记录本次移动的距离作为下次的beforemoveX
        _this.isAnimate = true
        if (_this.currmoveX > 30 || _this.currmoveX > -30 && _this.currmoveX < 0) {
          _this.distanceX = _this.beforemoveX = _this.width;
          _this.distanceX = _this.beforemoveX = _this.width;
          _this.isMsk = false
        } else if (_this.currmoveX < -30 || _this.currmoveX < 30 && _this.currmoveX > 0) {
          _this.distanceX = _this.beforemoveX = 0
          _this.isMsk = true
        }
      })
    },
    methods: {
      closeMenue(){
        this.distanceX = this.beforemoveX = 0
        this.isMsk = true
      },
    }
  }
</script>

<style scoped>
  .content .menue-content ul li {
    width: 100%;
    height: 60px;
    background-color: white;
    text-align: center;
    margin-bottom: 5px;
    line-height: 60px;
    font-size: 16px;
  }
</style>
