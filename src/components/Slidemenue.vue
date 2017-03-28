<template xmlns:v-bind="http://www.w3.org/1999/xhtml">
  <div class="slide-menue">
    <div class="menue" ref="slideMenu" :class="{menuetransition:isAnimate}">
      <slot></slot>
    </div>
    <div class="mask" ref="mask" @click="closeMask($event)"></div>
  </div>

</template>

<script>
  //poX:侧滑菜单的位置
  export default {
    name: 'slidemenue',
    data () {
      return {}
    },
    props: {
      isAnimate: Boolean, //动画过度启用时机，修改css中的.menue-transition
      posX: Number, //侧滑菜单滑动时的实时位置
      issmooths: Boolean, //菜单打开关闭平滑过度
      showMsk: Boolean,//遮罩层开关
      width:Number //侧滑菜单宽度
    },
    watch: {
      posX(val){
        var m = -this.width + val
        this.$refs.slideMenu.style.transform = 'translate3d(' + m + 'px,0,0)'
      },
      showMsk(val){
        if (val){
          this.$refs.mask.style.opacity=0
          this.$refs.mask.style.zIndex=-10
        }else {
          this.$refs.mask.style.opacity=1
        }
      }
    },
    methods: {
      closeMask(e){
        if (e.target.className == "mask") {
          this.$refs.mask.style.opacity=0
          this.$emit("close")
        }
      },
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .menue {
    width: 300px;
    height: 100%;
    background-color: #eee;
    position: absolute;
    top: 0;
    z-index: 10;
    transform: translate3d(-300px, 0, 0);
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
