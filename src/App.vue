<template>
  <div id="app">
    <slide-menue  :width="300">
      <div class="content">
        <div class="user-header">
          <img src="./components/images/image.jpg"/>
        </div>
        <div class="menue-content" @click="clicks($event)">
          <ul>
            <li>列表1</li>
            <li>列表2</li>
            <li>列表3</li>
          </ul>
        </div>
      </div>
    </slide-menue>
    <div></div>
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
      }
    },
methods:{
  clicks(e){
    alert(e.target.innerHTML)
  }
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
