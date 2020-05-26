<template>
<div id ="background">
<div id="floor" :class = "{hide: hideHome}"></div>
<div id="box-container" :class = "{hide: hideHome}" @mouseenter= "shakeBox()">
  <img src ="../assets/lid.svg" id="lid" @click= "openLid()">
  <img src = "../assets/box.svg" id="box">
</div>
  <div id="circle" :class= "{ zIndex: bringToFront, hide: hideHome}"></div>
  <sea :class= "{hide: hideSea}" class="sea" style= "opacity: 0"></sea>
</div>
</template>

<script>
import anime from 'animejs/lib/anime.es.js';
import sea from './sea.vue';
export default {
  name: 'home',
  props: {
    bringToFront:{
      type: Boolean,
      default: false
    },
    hideHome:{
      type: Boolean,
      default: false
    },
    hideSea:{
      type: Boolean,
      default: true
    }
    },

  methods: {
  openLid:  function () {  
    setTimeout(() => this.bringToFront = true, 3000); // easy fix for accesibility of bringToFront
    // const showSea =  () => this.hideHome = true;
    const timeline = anime.timeline({
      duration: 1000
    })
    timeline.add({  // to do: animate shadow too
    targets:'#lid',
    rotate: -180,
    translateX: '62vw',
    translateY: '-34vh',
    duration: 2000,
    easing: 'easeInOutExpo'
  }).add({
    targets: '#circle',
    translateY: '-35vh',
    translateX: '-10vw',
    easing: 'easeInOutBack',
    endDelay: 200
  }).finished.then(async function(){ // async function to be able to use .await in for loop
      let x = 1;
      let y = 1;
      let d = 100; //duration
      const colors = ['#68B77E','#5B9E6E','#498B69','#2C7C60','#156D61','#156A6A','#0F4C5A','#093145','#051D35','#000522'];
    for(let i = 0; i< 10; i++){ // build in anime loop executes parameter function only once, and then loops the returned value over and over again
      await anime({ 
      targets: '#circle',
      backgroundColor: colors[i],
      duration: function(){
        d = d + 20
        return d;
      } ,
      easing: 'easeInOutSine',
     scaleX: function(){
        const randomN = Math.random() * (2.5 - 0.5) + 0.5; //define max & min random
        const toReturn = x + randomN;
        if(randomN > 1.5){
          x = x  + 0.5
        }else{
          x = x + randomN;
        }
        return toReturn;
      },
      scaleY: function(){
          const randomN = Math.random() * (2.5 - 0.5) + 0.5; //define max & min random
          const toReturn = y + randomN;
          if(randomN > 1.5){
          y = y +0.5
          }else{
           y = y + randomN;
        }
          return toReturn;
      }
  }).finished //.finished to return a promise (needed for await)
  }
  }).then( () => {  // arrow function for different 'this' scope
    this.hideSea = false
    anime({
      targets: '.sea',
      opacity: 1
    })
  });
  },
  shakeBox: function(){
   anime({
      targets: '#lid, #box',
      rotate: 2,
      direction: 'alternate',
      loop: 6,
      autoplay: true,
      duration: 100,
      easing: 'easeInOutBounce'
    })
   
  }
},
components: {
  sea
}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
#background{
  width: 100vw;
  height: 100vh;
  background-color: #F2AD9F;
  position: absolute;
}

#floor{
  width: 100vw;
  height: 30vh;
  background-color: #F26968 ;
  position: absolute;
  bottom: 0;
}

#box-container{
  display: block;
  position: absolute;
  left: 50%;
  -webkit-transform: translateX(-50%);
  transform: translateX(-50%);
  z-index: 1;
  bottom: 20vh;
  img{
    max-width: 70vw;
    display: block;
    margin: 0 auto;
  }
  
}
#circle{
  width: 15vw;
  height: 15vw;
  position: absolute;
  top: 50%;
  left: 50%;
  border-radius: 300px;
  background-color: #6CBF84;
}
.zIndex{
  z-index: 2;
}
#lid{
  display: block;
  position: relative;
  top: 2vh;
  max-width: 73vw;
  cursor: pointer;
  z-index: 2;
}
.hide{
  display: none;
  .img{
    display: none;
  }
}
</style>
