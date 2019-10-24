<template>
	<div class="load-sets">
	    <button @click="PopUp">Load Sets</button>
		<transition-group name="modal">	
			<div key="pop-up-wrap" v-if="showModal" class="pop-up-wrap" @click="PopUp">
			</div>	
	        <div key="pop-up" v-if="showModal" class="pop-up">
	        	<div class="pop-up-title">
	        		Load Ambient Sets
	        	</div>
	        	<div class="pop-up-close d-flex"
	        		@click="PopUp">
	        		<img src="@/../static/icons/error.svg" alt="">
	        	</div>
	            <div class="set-group d-flex">
	                <div v-for="set in loadSetsData"
	                	 @click="LoadSet(set)"
	                	 class="set-item d-flex">
	                	 <div class="set-item-set-title">
	                    	{{ set.setTitle }}
	                	 </div>
	                    <div class="set-item-unit-group d-flex">
	                        <div v-for="unit in set.set" 
	                        	 class="set-item-unit-item" 
	                        	 :style="'background-color:' + unit.activColor">
	                            {{ unit.unitTitle }}
	                        </div>
	                    </div>
	                </div>
	            </div>
	        </div> 
    	</transition-group>
	</div>
</template>

<script>
const LoadSetsData = require('../assets/LoadSetsData.json');
export default {
	name: 'LoadSets',
    data(){
        return{
            loadSetsData: LoadSetsData,
            showModal: false
        }
    },
    methods:{
    	PopUp(){
    		this.showModal = !this.showModal;			
    	},
    	LoadSet(set){
    		this.$emit('loadSet', set.set);
    	}
    }
}
</script>

<style  scoped>
/* ====Disclaimer==== */
/*-----my style is bad------*/

.d-flex{
	display: -webkit-flex;
	display: -moz-flex;
	display: -ms-flex;
	display: -o-flex;
	display: flex;
}
.load-sets{
    margin-left: auto;
}
.load-sets button{
    background: #fff;
    border: none;
    border-radius: 30px;
    color: #000; 
    font-weight: bold;
    padding: 15px 20px;
    font-size: 14px;
    outline: none;
    cursor: pointer;
}
.pop-up-wrap{
    width: 100%;
    height: 100vh;
    background: rgba(18,17,18, 0.9);
    top: 0;
    left: 0;
    position: fixed;
    z-index: 3;
}
.pop-up{
    max-width: 800px;
    width: 90%;
    background: #15161C;
    border-radius: 15px;
    position: fixed;
    top: 50%;
    left: 50%;
    z-index: 4;
    transform: translate(-50%,-50%);
    padding-bottom: 50px;
    -webkit-box-shadow: 0px 0px 35px 0px rgba(0,0,0,0.75);
	-moz-box-shadow: 0px 0px 35px 0px rgba(0,0,0,0.75);
	box-shadow: 0px 0px 35px 0px rgba(0,0,0,0.75);
}
.pop-up-title{
	padding: 30px 0 30px 20px;
	text-align: left;
	font-size: 30px;
	color: #fff;
}
.pop-up-close{
	background: #fff;
	width: 18px;
	height: 18px;
	border-radius: 50%;
	align-items: center;
	justify-content: center;
	position: absolute;
	top: 10px;
	right: 10px;
	cursor: pointer;
	transition: transform .2s linear;
}
.pop-up-close img{
	width: 6px;
	height: 6px;
}
.pop-up-close:hover{
	transform: rotateZ(-180deg);
}
.set-group{
	flex-direction: column;
}
.set-item{
	cursor: pointer;
	width: 100%;
	padding: 10px 20px;
	color: #fff;
	align-items: center;
	border-bottom: 1px solid #333;
	border-top: 1px solid #333;
	transition: background .2s linear; 
}
.set-item:hover{
	background: #333;
}
.set-item-set-title{
}
.set-item-unit-group{
	margin-left: auto;
}
.set-item-unit-item{
	margin:0 3px;
	padding: 2px 10px;
	border-radius: 18px;
	color: #333;
}

/*Modal window*/
.modal-enter-active,
.modal-leave-active,
.modal-move {
  transition: 500ms cubic-bezier(0.59, 0.12, 0.34, 0.95);
  transition-property: opacity, transform;
}
.modal-enter {
  opacity: 0;
}

.modal-enter-to {
  opacity: 1;
}
.modal-leave-active {
}
.modal-leave-to {
  opacity: 0;
  transform: scaleY(0);
  transform-origin: center top;
}

/*==========
	MEDIA	
=========*/
@media screen and (max-width: 915px) {
  .load-sets button{
  	position: absolute;
    padding: 7px;
    top: 0px;
    right: 125px;
    border-radius: 0;
   	border-bottom-left-radius: 8px;
    border-bottom-right-radius: 8px;
  }
}
@media screen and (max-width: 575px) {
	/* .set-item{
		flex-direction: column;
	} */
	.set-item-unit-item,
	.set-item-set-title{
		font-size: 14px;
	}
	.set-item-unit-group{
		margin-left: auto;
	}
	/* .set-item-set-title{
		display: none;
	} */
	.set-item{
		padding: 2px 5px;
	}
}
@media screen and (max-width: 440px) {
	.pop-up-title{
		font-size: 24px;
	}
	.set-item-unit-item{
		font-size: 12px;
	}
	.set-item-unit-group{
		margin-left: 0;
		margin: 0 auto;  
	}
	.set-item-set-title{
		display: none;
	} 
	.set-item{
		padding: 2px 0;
	}
	.load-sets button{
		padding-bottom: 10px;
		font-size: 13px;
	}
}
@media screen and (max-width: 365px) {
	.load-sets button{
		font-size: 12px;
		right: 105px;
	}
}
</style>