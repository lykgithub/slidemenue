<template>
  <div id="app">
    <slide-menue :isAnimate="isAnimate" :width="width" :posX=distanceX :showMsk='isMsk' @close="closeMenue" @clickMenue="clickMenue($event)">
      <div class="content">
        <ul>
          <li>1</li>
          <li>2</li>
          <li>3</li>
        </ul>
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
        isMsk: true ,  //控制遮罩层的开关
        width:350
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
      window.document.addEventListener('touchstart', function (e) {
        _this.isAnimate = false
        _this.startX = e.changedTouches[0].pageX
      });
      window.document.addEventListener('touchmove', function (e) {
        e.preventDefault();
        _this.currmoveX = e.changedTouches[0].pageX - _this.startX;
      })
      window.document.addEventListener('touchend', function (e) {
        _this.beforemoveX = _this.distanceX;  //抬起手指记录本次移动的距离作为下次的beforemoveX
        _this.isAnimate = true
        if (_this.currmoveX > 100 || _this.currmoveX > -100 && _this.currmoveX < 0) {
          _this.distanceX = _this.beforemoveX = _this.width;
          _this.distanceX = _this.beforemoveX = _this.width;
          _this.isMsk = false
        } else if (_this.currmoveX < -100 || _this.currmoveX < 100 && _this.currmoveX > 0) {
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
      clickMenue(e){
        alert(e.target.innerHTML)
      }
    }
  }
</script>

<style>
 .content ul li {
   width: 100%;
   height: 30px;
   background-color: white;
   margin-bottom: 2px;
   text-align: center;
   margin-bottom: 5px;
 }
</style>
