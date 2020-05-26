<template lang="html">
<div id="backgroundSea" @mousemove= "customCursor">
<img src="../assets/zeeBodem.svg" id="zeebodem">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 320" id="waves">
<path fill="#F2AD9F" fill-opacity="1" d="M0,128L30,144C60,160,120,192,180,202.7C240,213,300,203,360,202.7C420,203,480,213,540,224C600,235,660,245,720,229.3C780,213,840,171,900,170.7C960,171,1020,213,1080,229.3C1140,245,1200,235,1260,213.3C1320,192,1380,160,1410,144L1440,128L1440,0L1410,0C1380,0,1320,0,1260,0C1200,0,1140,0,1080,0C1020,0,960,0,900,0C840,0,780,0,720,0C660,0,600,0,540,0C480,0,420,0,360,0C300,0,240,0,180,0C120,0,60,0,30,0L0,0Z">
</path></svg>
<img src="../assets/bubble.svg" v-for="bubble in bubbles" :key= "bubble.id" :class="bubble.size" :style="{left: randomX(), top: randomY()}">
<img src="../assets/boat.svg" id="boat">
<div id= "sky">
  <img src="../assets/reload.svg" @click= "startOver()">
</div>
<div id="cursor"></div>  
</div>

</template>

<script lang="js">
  import anime from 'animejs/lib/anime.es.js';
  let notPlaying = true; // trigger animation only when not already playing
  let bubblesCreated = false;
  let swimAnimation = anime({      // to only have one instance of the animation
          targets: '#backgroundSea',
          backgroundColor: '#4EBFC7',
          duration: 9000,
          autoplay: false
        });
  export default  {
    name: 'sea',
    props: {},
    data () {
      return {
        swimAnimation: swimAnimation,
        bubbles: []
      }
    },
    methods: {
      startOver: function(){
        location.reload();
      },
      randomX: function(){
        const viewportWidth = document.documentElement.clientWidth;
        return Math.round(Math.random() * (viewportWidth)) + 'px';
      },
      randomY: function(){
        const viewportHeight = document.documentElement.clientHeight;
        return Math.round(Math.random() * (viewportHeight)) + 'px';
      },
      customCursor: function(event){
        const cursorX = event.clientX;
        const cursorY = event.clientY;
        this.scrollOnEdge(cursorY)
        anime({
          targets: '#cursor',
          translateX: cursorX,
          translateY: cursorY,
        })
      },
      scrollOnEdge: function(cursorY){ // tutorial: https://github.com/bennadel/JavaScript-Demos/blob/master/demos/window-edge-scrolling/index.htm
        // get viewport height
        const viewportHeight = document.documentElement.clientHeight;
        // determine edge dimensions
        const edgeSize = 150;
        const edgeTop = edgeSize;
        const edgeBottom = viewportHeight - edgeSize;

        // create boolean to determine if cursor is in edge
        const isInTopEdge = ( cursorY < edgeTop );
        const isInBottomEdge = ( cursorY > edgeBottom );
        if(isInTopEdge && notPlaying){
          this.swimmingUpAnimation();
          this.bubblesUp();
          notPlaying = false;
        }
        if(!isInTopEdge && !notPlaying && !isInBottomEdge){
          //notPlaying = true;
        }
        // if(isInBottomEdge && notPlaying){  // unstable 
        //     this.swimmingUpAnimation(true);
        //     notPlaying = false;
        // }
        if(bubblesCreated == false){
          this.createBubbles();
          bubblesCreated = true;
        }
      },
      swimmingUpAnimation(){
      //  const reversing = function(){   // to do: fix bugginess
      //       console.log('hey');
      //       if(reverse){
      //         console.log ('reverse');
      //         return 'reverse'
      //       }else{
      //         console.log('normal')
      //         return 'normal'
      //     } }
        const animation = anime.timeline({  
          //direction: reversing(), //to do: solve mystery of unstable reversing
          easing: 'linear',
          autoplay: true,
        })
        animation.add({
          targets: '#zeebodem', // swipe out zeebodem
          translateY: '100vh',
          duration: 1200
        }, 0).add({ // make water lighter
          targets: '#backgroundSea',
          backgroundColor: '#4EBFC7',
          duration: 2000
        }, 200).add({ // swipe in waves
          targets: '#waves',
          display: 'block',
          translateY: '80vh',
          duration: 1000
        }, '-=500').add({
          targets: '#sky',
          translateY: '40vh'
        },'-=800').add({
          targets: '#boat',
          opacity: 1,
          duration: 10
        }, '-=300').add({
          targets:'#boat',
          translateX: '20vw',
          duration: 9000,
          easing: 'easeInOutQuad'
        }).add({
          targets: '#sky img',
          opacity: 1
        }, '-=7000')
      },
      createBubbles: function(){
        for(let i = 0; i < 44; i++ ){
          const bubbleObject = {
            id: i,
            size: '',
          }
          switch(i%6){
            case 0: bubbleObject.size = 'small';
                    break;
            case 1: bubbleObject.size = 'small';
                    break;
            case 2: bubbleObject.size = 'small';
                    break;
            case 3: bubbleObject.size = 'medium';
                    break;
            case 4: bubbleObject.size = 'medium';
                    break;
            case 5: bubbleObject.size = 'large';
                    break;
            default: bubbleObject.size = 'small';
                     break;
          }
          console.log(bubbleObject);
          this.bubbles.push(bubbleObject);
        }
      },
      bubblesUp: function(){
        const animation = anime.timeline({
          targets: '.small, .medium, .large',
          easing: 'easeInOutSine'
        })
        animation.add({
          opacity: 1,
          translateY: '-45vh',
          duration: 3210,
        }).add({
          opacity: 0,
          //translateY: '-10vh',
          duration: 1000,
        }, '-=500')

      }
    }
}

</script>

<style scoped lang="scss">

  #backgroundSea{
    width: 100vw;
    height: 100vh;
    position: absolute;
    background-color: #000522;
    z-index: 3;
    cursor: none; //image resoslution of css custom cursor too small
  }
  #zeebodem{
    position: absolute;
    width: 100vw;
    bottom: 0;
    z-index: 1;
  }
  #waves{
    position: absolute;
    width: 100vw;
    top: -50vh;
  }
  #cursor{
    background-image: url("../assets/seahorse.svg");
    position: fixed; // cursor is placed relative to the viewport
    width: 4vw;
    height: 8vh;
    background-size: contain;
    background-repeat: no-repeat; 
  }
  #sky{
    background-color: #F2AD9F;
    width: 100vw;
    height: 45vh;
    top: -45vh;
    position: absolute;
    cursor: auto;
    img{
      position: relative;
      width: 5vw;
      left: 80vw;
      top: 10vh;
      opacity: 0;
      display: block;
      cursor: pointer;
    }
  }
  #boat{
    position: absolute;
    bottom: 40vh;
    left: -5vw;
    width: 15vw;
    opacity: 0;
    z-index: 3;
  }

  .small{
    width: 1vw;
  }

  .medium{
    width: 2vw;
  }

  .large{
    width: 3vw;
  }
  .small, .medium, .large{
    position: absolute;
    z-index: -1;
    opacity: 0;
  }
</style>
