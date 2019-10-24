<template>
  <div class="box-item">
     <div class="flip-box">
      <div class="preloader"></div>
      <div class="flip-box-front text-center" :style="getBackgroundImg">
        <div class="item-bg" :style="getBackgroundFrontEmpty">
          <div class="inner">
            <p :style="getColorTitle">{{ unitObj.title }}</p>
          </div>
        </div>
      </div>
      <div class="flip-box-back text-center" :style="getBackgroundImg" @click="Play">
        <div class="item-bg" :style="getBackgroundBackEmpty">
          <div class="inner">
            <img src="@/../static/icons/musical-note.svg" alt="musical-note">
          </div>
        </div>
      </div>
     </div>
  </div>
</template>

<script>
import {Howl, Howler} from 'howler';
export default {
  name: 'ElpyUnit',
  props: {
    unitObj: Object
  },
  data () {
    return {
      sound: new Howl({
        src: ['@/../static/audio/' + this.unitObj.sounds ],
        loop: true
      })
    }
  },
   computed: {
    isVolume(){
       return this.unitObj.primaryVolume;
    },
     /*Картинка на фоне DOM = item */
    getBackgroundImg() {
      return 'background-image: url(' + '@/../static/images/' + this.unitObj.bgImage + '); background-size: cover; background-repeat: no-repeat; background-position: center;';
    },
     /*Затемнение фона Front DOM = item__bg*/
    getBackgroundFrontEmpty(){
      return (this.isAudioPlay === 'play' || this.isAudioPlay === 'pause') ?
             ('background: ' + this.unitObj.activColor + '; opacity: 0.6;') :
              'background: rgba(0,0,0,0.6);';
    },
     /*Затемнение фона Back DOM = item__bg*/
    getBackgroundBackEmpty(){
      return (this.isAudioPlay === 'play' || this.isAudioPlay === 'pause') ?
             ('background: ' + this.unitObj.activColor + '; opacity: 0.3;') :
              'background: rgba(0,0,0,0.3);';
    },
    getColorTitle(){
      return (this.isAudioPlay === 'play' || this.isAudioPlay === 'pause') ? 'color: #000' : 'color: #fff';
    },
    /*Тени DOM = item__shadow*/
    /*getHoverShadow(){
      return (this.isAudioPlay === 'play' || this.isAudioPlay === 'pause')  ? ('box-shadow:  0px 0px 35px 0px ' + this.unitObj.activColor + '; -webkit-box-shadow: 0px 0px 35px 0px ' + this.unitObj.activColor + '; -moz-box-shadow: 0px 0px 35px 0px ' + this.unitObj.activColor + ';') : false;
    },*/
    isAudioPlay(){
      return this.unitObj.audioPlay;
    }
  },
  watch:{
    isAudioPlay(){
      if(this.isAudioPlay === "play")
          this.sound.play();
      if(this.isAudioPlay === "pause")
          this.sound.pause();
      if(this.isAudioPlay === "stop")
          this.sound.stop();    
    },
    isVolume(){
      this.sound.volume(this.isVolume);
    }
  },
  methods: {
    Play(){
      this.$emit('selectUnit', this.unitObj);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* ====Disclaimer==== */
/*-----my style is bad------*/

/*================*/
.box-item {
  position: relative;
  -webkit-backface-visibility: hidden;
  max-width: 180px;
  margin-bottom: 35px;
  width: 50%;
  margin: 5px;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -o-user-select: none;
  user-select: none;
}

.flip-box {
  -ms-transform-style: preserve-3d;
  transform-style: preserve-3d;
  -webkit-transform-style: preserve-3d;
  perspective: 1000px;
  -webkit-perspective: 1000px;
}
/*preloader*/
.preloader {
  border-top: 2px solid #d0d9df; /* Blue */
  border-radius: 50%;
  width: 80%;
  height: 80%;
  position: absolute;
  top: 50%;
  left: 50%;
  /* z-index: 1; */
  animation: spin 1.5s linear infinite;
}

@keyframes spin {
  0% {
    transform: translate(-50%,-50%) rotate(0deg);
  }
  100% {
    transform: translate(-50%,-50%) rotate(360deg);
  }
}

.flip-box-front,
.flip-box-back {
  background-size: cover;
  background-position: center;
  border-radius: 10px;
  height: 180px;
  -ms-transition: transform 0.7s cubic-bezier(.4,.2,.2,1);
  transition: transform 0.7s cubic-bezier(.4,.2,.2,1);
  -webkit-transition: transform 0.7s cubic-bezier(.4,.2,.2,1);
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
  z-index: 2;
}
.flip-box-front {
  position: relative;
  -ms-transform: rotateY(0deg);
  -webkit-transform: rotateY(0deg);
  transform: rotateY(0deg);
  -webkit-transform-style: preserve-3d;
  -ms-transform-style: preserve-3d;
  transform-style: preserve-3d;
}


.flip-box:hover .flip-box-front {
  -ms-transform: rotateY(-180deg);
  -webkit-transform: rotateY(-180deg);
  transform: rotateY(-180deg);
  -webkit-transform-style: preserve-3d;
  -ms-transform-style: preserve-3d;
  transform-style: preserve-3d;
}
.flip-box-back {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  
  -ms-transform: rotateY(180deg);
  -webkit-transform: rotateY(180deg);
  transform: rotateY(180deg);
  -webkit-transform-style: preserve-3d;
  -ms-transform-style: preserve-3d;
  transform-style: preserve-3d;
}
.flip-box:hover .flip-box-back {
  -ms-transform: rotateY(0deg);
  -webkit-transform: rotateY(0deg);
  transform: rotateY(0deg);
  -webkit-transform-style: preserve-3d;
  -ms-transform-style: preserve-3d;
  transform-style: preserve-3d;
}
.item-bg{
  width: 180px;
  height: 180px;
  border-radius: 10px;
}
.flip-box .inner {
  position: absolute;
  left: 0;
  width: 100%;
  padding: 5px;
  outline: 1px solid transparent;
  -webkit-perspective: inherit;
  perspective: inherit;
  z-index: 2;
  
  transform: translateY(-50%) translateZ(60px) scale(.94);
  -webkit-transform: translateY(-50%) translateZ(60px) scale(.94);
  -ms-transform: translateY(-50%) translateZ(60px) scale(.94);
  top: 50%;
}
.flip-box .inner img{
  width: 100px;
}
.flip-box p {
  font-family: 'Lexend Deca', sans-serif;
  font-size: 20px;
  line-height: 1.5em;
}


/*==========
    MEDIA   
=========*/
@media screen and (max-width: 445px) {
  .box-item{
    max-width: 135px;
  }
  .flip-box-front,
  .flip-box-back{
    height: 135px;
  }
  .item-bg{
    width: 135px;
    height: 135px;
  }
}
</style>