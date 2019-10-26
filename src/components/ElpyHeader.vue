<template>
<div>
    <header class="header">
        <div class="author d-flex">
          ElpyWeb by Admin 
          <a href="https://github.com/TheAdminPro/ElpyWeb" target="_blank">
            <img src="@/../static/icons/github.svg" alt="github-logo">
          </a>
        </div>
        <Timer @endTime="EndTime"/>
        <nav class="nav d-flex">
            <div class="nav-wrap d-flex">
                <div class="master-volume d-flex">
                    <button class="master-volume-btn d-flex"
                            @click="TotalPause" 
                            :disabled="isTotalPause"
                            :class="{disabledBG: isTotalPause}"
                            >
                        <img v-if="isAudioPlay" src="@/../static/icons/pause.svg" alt="pause" class="icon-pause">
                        <img v-else src="@/../static/icons/play.svg" alt="pause" class="icon-pause">
                    </button>
                    <div class="master-volume-change">
                        <div class="master-volume-title">
                            Master Volume
                        </div>
                        <input type="range" class="master-volume-range"
                               value="2"
                               min="0"
                               max="10"
                               step="0.1"
                               v-model="MasterVolume"
                               @input="ChangeTotalVolume">
                    </div>
                </div>
                <!--Group Ranges-->
                <div class="sub-group-ranges d-flex">
                    <transition-group tag="div" name="list" class="d-flex">
                       <div class="sub-item" v-for="(unit, index) in selectedUnit" :key="unit._id">
                        <!-- debugging {{  unit.primaryVolume * 10 }} -->
                           <div class="sub-item-title d-flex">
                               <div class="sub-item-text" 
                                    :style="'color:' + unit.activColor + '; text-shadow: 0px 0px 5px' + unit.activColor">
                                   {{ unit.title }}
                                   <!-- debugging {{ subordinateVolume }} -->
                               </div>   
                               <div class="sub-item-btn">
                                        <img src="@/../static/icons/close.svg" alt="Close" 
                                             @click="CloseUnit(unit, index)">
                                </div>
                           </div>
                               <input type="range" class="sub-item-range" 
                                      value="2" 
                                      min="0"
                                      max="10"
                                      step="0.1" 
                                      v-model="subordinateVolume[index]" 
                                      @input="ChangeSubordinateVolume(unit, index)"
                                      :style="'background-color:' + unit.activColor+'; box-shadow:  0px 0px 10px 0px ' + unit.activColor">
                       </div>
                    </transition-group>
                   <!-- Default -->
                   <div class="sub-item" v-for="(unit, index) in isDefaultUnit" disabled :key="index">
                        <div class="sub-item-title d-flex">
                           <div class="sub-item-text default-sub-item-text">
                               Free Place #{{ selectedUnit.length + index + 1 }}
                           </div>   
                       </div>
                           <input type="range" value="5" min="0" max="10" class="sub-item-range default-sub-item-range" disabled>
                   </div>
                </div>
            </div>
            <!--End Group Ranges-->
            
            <!--Component LoadSets--->
            <LoadSets @loadSet="LoadSet"/>
            <!--Component LoadSets--->

        </nav>
    </header>
    <!-- -->
</div>
</template>

<script>
import LoadSets from './LoadSets.vue';
import Timer from './Timer.vue';
export default {
    name: 'ElpyHeader',
    components: {
        LoadSets,
        Timer
    },
    props: {
        selectedUnit: Array,
        isAudioPlay: Boolean
    },
    data(){
        return{
            MasterVolume: 3,
            subordinateVolume: []
        }
    },
    computed: {
        isTotalPause(){
            return this.selectedUnit.length === 0;
        },
        isDefaultUnit(){
            return 3 - this.selectedUnit.length;
        }
    },
    methods: {
        ChangeTotalVolume(){
            this.$emit('changeTotalVolume', this.MasterVolume);
        },
        TotalPause(){
            this.$emit('totalPause');  
        },
        ChangeSubordinateVolume(unit, index){
            this.$emit('changeSubordinateVolume', {unit: unit, volume: this.subordinateVolume[index]});
        },
        CloseUnit(unitObj, index){
            this.$emit('closeUnit', unitObj);
        },
        LoadSet(set){
            this.$emit('loadSet', set);
        },
        EndTime(){
            this.$emit('endTime');
        }
    },
    /* RU: <-Обновление состояния громкости звуков id->
       EN: <-Sound Volume Status Update->
    */
    beforeUpdate(){
        this.selectedUnit.forEach((unit, index)=>{
            this.subordinateVolume[index] = (unit.primaryVolume * 10);
        });
    }
}
</script>

<style scoped>
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
/*================
       HEADER
=================*/
.author{
  /* border-bottom: 1px solid #333; */
  width: 100%;
  max-width: 1440px;
  margin: 0 auto;
  padding:2px 0 2px 20px;
  color: #fff;
  font-size: 12px;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -o-user-select: none;
   user-select: none;
   align-items: center;
}
.author img{
  width: 20px;
  margin-left: 10px;
}
.header{
    background: #14151C;
    width: 100%;
    margin: 0 auto;
    height: 130px;  
    position: fixed;
    z-index: 999;
   -webkit-touch-callout: none;
   -webkit-user-select: none;
   -khtml-user-select: none;
   -moz-user-select: none;
   -ms-user-select: none;
   -o-user-select: none;
    user-select: none;
    -webkit-box-shadow:  0px 0px 15px 0px rgba(0,0,0,0.75);
-moz-box-shadow:  0px 0px 15px 0px rgba(0,0,0,0.75);
box-shadow: 0px 0px 15px 0px rgba(0,0,0,0.75);
}
.nav{
    width: 100%;
    margin: 0 auto;
    background: #14151C;
    max-width: 1440px;
    padding: 15px 20px;
    color: #C9C9CB;
   font-family: 'Lexend Deca', sans-serif;
    align-items: center;
    flex-wrap: wrap;
    /* justify-content: space-between; */
}
.master-volume{
    align-items: center;
    justify-content: center;
    margin-right: auto;
}
.master-volume-btn{
    width: 60px;
    height: 60px;
    border-radius: 50%;
    background: #fff;
    border:none;
    margin-right: 15px;
    outline: none;
    cursor: pointer;
    align-items: center;
    justify-content: center;
}
.master-volume-btn:focus{
    outline: none;
}
.master-volume-btn img{
    width: 20px;
    height: 20px;
    vertical-align: middle;
}
.master-volume-title{
    text-align: left;
    margin-left: 5px;
    font-weight: bold;
    font-size: 15px;
}
.sub-group-ranges{
    border-right: 1px solid #333;
    border-left: 1px solid #333;
    padding: 0 10px; 
    margin: 0 20px;
    /* margin-right: auto; */
}
.sub-item{
    margin: 0 10px;
}
.sub-item-title{
    align-items: center;
    justify-content: space-between;
}
.default-sub-item-text{
    color: #7A7A7B;
}
.sub-item-btn{
    margin-left: 10px;
}
.sub-item-btn img{
    width: 10px;
    height: 10px;
}
.sub-item-btn img:hover{
    cursor: pointer;
}
.disabledBG{
    background: #d3d3d3 !important;
    cursor: inherit !important;
}
/*Range Style*/
.master-volume-range,
.subordinate-range {
    overflow: hidden;
    width: 150px;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    background-color: #333;
    border-radius: 10px;
    outline: none;
}
.master-volume-range::-webkit-slider-runnable-track,
.sub-item-range::-webkit-slider-runnable-track{
    height: 20px;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    color: #fff;
}
.master-volume-range::-webkit-slider-thumb,
/*.master-volume-range::-moz-range-thumb,*/
.sub-item-range::-webkit-slider-thumb
/*.sub-item-range::-moz-range-thumb*/{
    width: 0px;
    height: 0px;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    cursor: ew-resize;
    box-shadow: -100px 0 0 100px #fff;
}
.master-volume-range::-moz-range-thumb{
    width: 0px;
    height: 0px;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    cursor: ew-resize;
    box-shadow: -100px 0 0 100px #fff;
}
.sub-item-range::-webkit-slider-thumb,
.sub-item-range::-moz-range-thumb{
    width: 0px;
    height: 0px;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    cursor: ew-resize;
    box-shadow: -65px 0 0 65px #fff;
}
/* .master-volume-range::-moz-range-progress,
.sub-item-range::-moz-range-progress {
    background-color: #fff;
} */
/* .master-volume-range::-moz-range-track,
.sub-item-range::-moz-range-track {
    background-color: #333;
} */
.master-volume-range::-ms-fill-lower,
.sub-item-range::-ms-fill-lower {
    background-color: #fff;
}
.master-volume-range::-ms-fill-upper,
.sub-item-range::-ms-fill-upper {
    background-color: #333;
}
.sub-item-range{
    overflow: hidden;
    width: 130px;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    background-color: #333;
    border-radius: 8px;
    outline: none;
}
/*transition-group*/
.list-enter-active,
.list-leave-active,
.list-move {
  transition: 500ms cubic-bezier(0.59, 0.12, 0.34, 0.95);
  transition-property: opacity, transform;
}

.list-enter {
  opacity: 0;
  transform: translateX(50px) scaleY(0.5);
}

.list-enter-to {
  opacity: 1;
  transform: translateX(0) scaleY(1);
}

.list-leave-active {
  position: absolute;
}

.list-leave-to {
  opacity: 0;
  transform: scaleY(0);
  transform-origin: center top;
}


/*==========
    MEDIA   
=========*/
@media screen and (max-width: 765px) {
  .header{
    height: 155px;
  }
  .sub-group-ranges{
    /* flex-direction: column; */
    width: max-content;
    margin: 0 auto;
    border: none;
    padding: 0;
  }
  .master-volume{
    align-items: flex-start;
  }
  .master-volume-btn{
    width: 45px;
    height: 45px;
  }
  .nav-wrap{
    flex-direction: column;
  }
  .sub-item{
    margin: 5px 8px 0 0;
  }
}
@media screen and (max-width: 445px) {
    .sub-item{
        margin-top: 0;
        align-self: flex-end;
    }
    .sub-item-text, 
    .default-sub-item-text{
        font-size: 12px;
    }
    .sub-item-range,
    .default-sub-item-range{
        width: 95px;
    }
    .nav{
        padding: 10px;
        padding-top: 15px;
    }
    .sub-group-ranges{
        margin-top: 5px;
    }
    .author{
        padding-left: 10px;
    }
    .sub-item-btn{
        margin-left: 3px;
    }
    .sub-item-btn img{
        width: 5px;
        height: 5px;
    }
    .author{
        font-size: 10px;
    }  
    .author img{
        width: 15px;
        height: 15px;
        margin-left: 5px;
    }
}
@media screen and (max-width: 330px) {
    .nav,.author{
        padding-left: 5px;
    } 
}
</style>