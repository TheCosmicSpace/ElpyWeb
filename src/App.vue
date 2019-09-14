<template>
  <div id="app">
    <ElpyHeader 
        @changeTotalVolume="TotalVolume"
        @changeSubordinateVolume="SubordinateVolume"
        @totalPause="TotalPause"
        @closeUnit="SelectUnit"
        @loadSet="LoadSet"
        @endTime="EndTime"
        :isAudioPlay="isAudioPlay"
        :selectedUnit="selectedUnit"/>    

    <section class="main">
      <div class="main-wrap">
        <div class="unit-group">
          <ElpyUnit v-for="(unit, index) in elpyUnitData"
              :key="unit._id"
              :unitObj="unit"
              @selectUnit="SelectUnit"/>
        </div>
      </div>
    </section>

  </div>
</template>

<script>
import Vue from 'vue'
/*import Howler*/
import {Howl, Howler} from 'howler';
import ElpyUnit from './components/ElpyUnit';
import ElpyHeader from './components/ElpyHeader';
/*import JOSN*/
import ElpyUnitData from './assets/ElpyUnitData.json';
export default {
  name: 'App',
  components: {
    ElpyUnit,
    ElpyHeader
  },
  data (){
    return {
      elpyUnitData: ElpyUnitData,
      selectedUnit: [],
      isAudioPlay: true
    }
  },
  methods:{
    /* RU: <-Изменение общей громкости->
       EN: <-Change the total volume->
    */
    TotalVolume(volue){
      let _volume = volue / 10;
      Howler.volume(_volume);
    },
    /* RU: <-Изменение громкости звуков->
       EN: <-Change the volume of sounds->
    */
    SubordinateVolume(payload){        
        let volume = +payload.volume / 10;
        this.elpyUnitData.findIndex((unit, index) => {
            unit._id == payload.unit._id ? ( unit.primaryVolume = volume )  : false;
        });
    },
    TotalPause(){
      this.isAudioPlay = !this.isAudioPlay;
      this.selectedUnit.forEach( unit => {
          this.isAudioPlay ? unit.audioPlay = 'play' : unit.audioPlay = 'pause';
      });
    },
    /* RU: <-Запуск выбранных звуков->
       EN: <-Launch selected sounds->
    */
    SelectUnit(unitObj){
        /* RU: <-Проверка на наличие в массиве->
           EN: <-Check for array presence->
        */
        let ind = this.selectedUnit.findIndex(unit => {
            return unit._id == unitObj._id;
        });
        /* RU: <-Добавление в массиве и запуск звука->
           EN: <-Adding in an array and starting sound->
        */
        if (ind === -1 || ind === undefined) {
          /* RU: <-Проверка лимита воспроизведенных звуков->
             EN: <-Checking the limit of sounds played->
          */
          if(this.selectedUnit.length < 3){
              this.selectedUnit.push(unitObj);
              this.isAudioPlay ? unitObj.audioPlay = "play" : unitObj.audioPlay = "pause";
          }else{  
              alert("Max Sound 3");
          }
        }
        /* RU: <-Остановка воспроизведения звука и удаление с массива->
           EN: <-Stop playing sound and remove from array->
        */
        else{
            unitObj.audioPlay = "stop";
            this.selectedUnit.splice(ind, 1);
        }
    },
    /* RU: <-Загрузка набора звуков->
       EN: <-Load sets of sounds->
    */
    LoadSet(set){
      let _set = [];

      this.selectedUnit.forEach(unit =>{
          unit.audioPlay = "stop";
      });

      set.forEach(setUnit => {
        this.elpyUnitData.forEach(unit => {
          if (unit.title == setUnit.unitTitle) {
            unit.primaryVolume = setUnit.volume;
            /*->?*/this.isAudioPlay ? unit.audioPlay = "play" : unit.audioPlay = "pause";
            _set.push(unit);

          }
        });
      });
      this.selectedUnit = _set;
    },
    /* RU: <-Срабатывание таймера->
       EN: <-Timer operation->
    */
    EndTime(){
      this.isAudioPlay = false;
      this.selectedUnit.forEach( unit => {
          unit.audioPlay = 'pause';
      });
    },
    /* RU: <-Добавление реактивных полей->
       EN: <-Add reactive fields->
    */
    initElpyUnitData(){
      this.elpyUnitData.forEach((unit) => {
        Vue.set(unit, 'primaryVolume', 0.2);
        Vue.set(unit, 'audioPlay', "stop");
        Vue.set(unit, '_id', this.idGenerator());
      });
    },
    /* RU: <-Генерация id->
       EN: <-Generation id->
    */
    idGenerator(min=48, max=126, len=10){
      let _id = '';
      for(let i = 0; i < len; i++){
          _id += String.fromCharCode(Math.floor(Math.random() * (max - min)) + min);
      }
      return _id;
    }
  },
  created(){
    this.initElpyUnitData();
  }
}
</script>

<style>
/* ====Disclaimer==== */
/*-----my style is bad------*/

/*root*/
.d-flex{
  display: -webkit-flex;
  display: -moz-flex;
  display: -ms-flex;
  display: -o-flex;
  display: flex;
}
*{
  margin: 0;
  padding: 0;
}
h1,h2,h3,h4,h5,h6{
  margin: 0;
  padding: 0;
}
#app {
  font-family: 'Lexend Deca', 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background: #15161C;
  width: 100%;
  min-height: 100vh;
  height: 100%;
  padding-bottom: 30px;
}
.main{
  background: #15161C;
  min-height:100vh;
  height: 100%;
  width: 100%;
  padding-top: 180px;
}
.main-wrap{
  width: 100%;
  max-width: 1440px;
  margin: 0 auto;
  padding: 0 10px;
}
.unit-group{
  display: -webkit-flex;
  display: -moz-flex;
  display: -ms-flex;
  display: -o-flex;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
