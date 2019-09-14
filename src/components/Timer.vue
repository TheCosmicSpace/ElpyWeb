<template>
	<div>
		<div v-if="timerNotNull" class="output-timer">
			<div class="close-timer">
				<img src="@/../static/icons/error.svg" alt="Close" @click="CloseTimer">
			</div>
			<div class="timer">{{getTime}}</div>
		</div>
		<img src="@/../static/icons/clock.svg" alt="Clock" class="timer-btn" @click="PopUp">
		<transition-group name="modal">
			<div key="pop-up-wrap" v-if="showModal" class="pop-up-wrap" @click="PopUp">
			</div>
			<div key="pop-up" v-if="showModal" class="pop-up">
		    	<div class="pop-up-title">
		    		Please select one to start an ambient timer
		    	</div>
		    	<div class="pop-up-close d-flex"
		    		@click="PopUp">
		    		<img src="@/../static/icons/error.svg" alt="Close">
		    	</div>
		        <div class="set-group d-flex">
		            <div v-for="time in timeArray"
		            	 @click="TimerClick(time)"
		            	 class="set-item d-flex">
		            	 {{ time }}
		            	 <span class="mins">mins</span>
		            </div>
		        </div>
		    </div>
	    </transition-group> 
	</div>
</template>

<script>
export default {
    name: 'Timer',
    data (){
    	return {
 	  		showModal: false,
 	  		timeArray: [15, 30, 45, 60, 90, 120],
 	  		timer: null,
 	  		min: 0,
 	  		sec: 0
 	  		/*call: new Audio("@/../static/audio/call.wav")*/
    	}
    },
    methods: {
    	/* RU: <-Работа Таймера->
           EN: <-Timer Operation->
        */
    	TimerClick(time){
    		this.PopUp();
    		this.timerNotNull ? clearInterval(this.timer) : this.timer = null;
    		this.min = time;
    		this.sec = 0;
    		this.timer = setInterval(()=>{
    			this.sec--;
    			if(this.sec < 0){
    				this.min--;
    				if(this.min < 0){
    					this.CloseTimer();
    					/*this.call.play();*/
    					this.$emit('endTime');
    				}else{
    					this.sec = 59;
    				}
    			}
    		}, 1000);
    	},
    	/* RU: <-Очистка таймера->
	       EN: <-Clear Timer->
	    */
    	CloseTimer(){
    		clearInterval(this.timer);
    		this.timer = null;
    		this.min = 0;
    		this.sec = 0;
    	},
    	PopUp(){
    		this.showModal = !this.showModal;			
    	},
    },
    computed: {
		getTime () {
			 return(this.min < 10 ? '0' + this.min : this.min) + 'm '+ (this.sec < 10 ? '0' + this.sec : this.sec)+'s';
		},
		timerNotNull(){
			return this.timer !== null;
		}
	},
}
</script>

<style scoped>
/* ====Disclaimer==== */
/*-----my style is bad------*/

.timer-btn{
	width: 20px;
	position: absolute;
	right: 20px;
	top: 10px;
	cursor: pointer;
}
.output-timer{
	background: #FFCA57;
	position: absolute;
	right: 50px;
	padding: 0px 6px;
	top: 0px;
	height: 55px;
	width: 70px;
	border-bottom-left-radius: 8px;
	border-bottom-right-radius: 8px;
	transition: all .2s linear; 
	transform: translateY(-20px);
}
.output-timer:hover{
	transform: translateY(0px);	
}
.close-timer img{
	width: 10px;
	cursor: pointer;
}
.timer{
	color: #000;
	font-size: 12px;
	padding-top: 5px;
}
.pop-up-wrap{
    width: 100%;
    height: 100vh;
    background: rgba(18,17,18, 0.9);
    top: 0;
    left: 0;
    position: fixed;
}
.pop-up{
    max-width: 800px;
    width: 80%;
    border-radius: 15px;
    position: fixed;
    background: rgba(18,17,18, 0.8);
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    padding: 150px 0;
	-webkit-box-shadow: 0px 0px 35px 0px rgba(0,0,0,0.75);
	-moz-box-shadow: 0px 0px 35px 0px rgba(0,0,0,0.75);
	box-shadow: 0px 0px 35px 0px rgba(0,0,0,0.75);
}
.pop-up-title{
	padding: 0 10px;
	margin-bottom: 40px;
	text-align: center;
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
	justify-content: center;
}
.mins{
	margin-top: auto;
	font-size: 14px;
	margin-left: 2px;
}
.set-item{
	cursor: pointer;
	padding: 10px 20px;
	color: #fff;
	align-items: center;
	border: 1px solid #333;
	border-radius: 28px;
	transition: background .2s linear;
	margin: 0 5px; 
	font-size: 25px;
}
.set-item:hover{
	background: #333;
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
  .set-group{
  	flex-direction: column;
    width: max-content;
    margin: 0 auto;
  }
  .set-item{
  	margin-bottom: 10px;
  }
  .pop-up{
  	padding: 30px 0;
  	z-index: 1;
  }
  .pop-up-title{
  	font-size: 24px;	
  }
}
@media screen and (max-width: 365px) {
	.timer-btn{
		right: 10px;
	}
	.output-timer{
		right: 33px;	
	}
}

</style>