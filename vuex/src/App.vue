<template>
  <div id="app">
    <!-- <router-view/> -->
    <!-- <head-box></head-box> -->
    <hello-world></hello-world>
    <!-- <foot-box></foot-box> -->
  </div>
</template>

<script>
  // import headBox from '@/components/headBox'
  import helloWorld from '@/components/HelloWorld'
  // import footBox from '@/components/footBox'
  import Vue from 'vue'
  // let bus = new Vue({})
  Vue.directive('swipe', {
    bind: function (el, bindings) {
      let start, move, stime, mtime, {arg, value} = bindings
      el.addEventListener('touchstart', (e) => {
        start = e.touches[0].pageX
        stime = new Date().getTime()
      })
      el.addEventListener('touchend', (e) => {
        move = e.changedTouches[0].pageX
        mtime = new Date().getTime()
        if (Math.abs(move - start) > 30 && move - start < 0 && arg === 'left' && mtime - stime < 1000) {
          console.log('左划')
          value()
        } else if (Math.abs(move - start) > 30 && move - start > 0 && arg === 'right' && mtime - stime < 1000) {
          console.log('右 划')
          value()
        }
      })
    }
  })
  export default {
    components: {
      // footBox,
      // headBox,
      helloWorld
    }
  }
</script>

<style>
  html, body, #app {
    width: 100%;
    height: 100%;
  }
  body {
    font-size: .32rem;
    color: #2c3e50;
  }
  #app {
    /* display: -webkit-flex;
    flex-direction: column; */
  }
</style>
