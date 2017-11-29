<template>
  <div id="app">
    <!-- <img src="./assets/logo.png">
    <router-view/> -->
    <swiper3d :list="list" :cur-width="curWidth" :all-width="allWidth" :all-height="allHeight" :cur-height="curHeight" :scale="0.8"></swiper3d>
  </div>
</template>

<script>
import swiper3d from './components/Swiper-3d'

function getViewportSize () {
  return {
    width: window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth,
    height: window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight
  }
}
let SIZE = getViewportSize()
export default {
  name: 'app',
  components: {swiper3d},
  data () {
    return {
      allWidth: SIZE.width,
      curWidth: SIZE.width / 1.2,
      allHeight: SIZE.height / 4.27,
      curHeight: SIZE.height / 4.27 * 0.95,
      list: [
        {content: 1, color: 'red'},
        {content: 2, color: 'green'},
        {content: 3, color: 'blue'},
        {content: 4, color: '#66CC66'},
        {content: 5, color: '#666666'}
      ]
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
</style>
