<template>

    <div class="audio-wrapper">
        <div :class="[{animateContent: animation === 1}, 'player-box']">
            <span class="player-box__favorite" v-if="!currentStation == '' && !change.favorite" @click="favorite()"><i class="far fa-star fa-fw" aria-hidden="true"></i></span>
            <span class="player-box__favorite" v-if="!currentStation == '' && change.favorite" @click="favorite()"><i class="fas fa-star fa-fw" aria-hidden="true"></i></span>
            <div class="player-box__textarea">
                <p class="player-box__title">{{streamName}}</p>
                <p class="player-box__title player-box__title--subline" v-if="!currentStation == ''">Stream</p>
                <p class="player-box__title player-box__title--subline" v-if="!currentStation == ''">{{streamCountry}}</p>
                <p class="player-box__title player-box__title--subline" v-if="currentStation == ''">{{message}}</p>
            </div>
            <div class="player" id="player">
                <button class="player__control-button" @click="playerPlay()" v-if="!isPlaying"><i class="fas fa-play fa-fw" aria-hidden="true"></i></button>
                <button class="player__control-button" @click="playerPause()" v-if="isPlaying"><i class="fas fa-pause fa-fw" aria-hidden="true"></i></button> 
                <div class="options">
                    <button class="options__list-button"  @click="animateContent()"><i class="fas fa-sort-down fa-fw" aria-hidden="true"></i></button>   
                    <p class="player-box__title player-box__title--subline">{{currentTime}}</p>
                </div>            
            </div>
            <img class="eq" src="../assets/eq2.gif" v-if="isPlaying"/>
        </div>
        <div :class="[{animateContent: animation === 1}, 'stations-list']">
            <stationsList :callMethod="changeStation" />
        </div>
        <p class="sign">Created by Piotr Bartkowicz <a href="https://github.com/BartkowiczPiotr">GitHub</a></p>
    </div>

</template>

<script>

import stationsList from '@/components/stationsList.vue';
import { setTimeout } from 'timers';

export default {

    name:'player',
    components:{

        stationsList,

    },
    data(){

        return{

        currentStation: '',
        message:'select the radio station you want to listen to',

        change: null,
        streamName: '',
        streamCountry:'',
        player: null,
        currentTime: '0:00', 
        isPlaying: false,
        animation: 0,
        timeOut: null,

        }

    },
    methods:{

        playerInit(){
            
            this.player = new Audio();
            this.player.controls = true;
            this.player.style.display = 'none';

            document.getElementById('player').appendChild(this.player);

            this.updateCurrentTime();

        },

        handleMediaError(e){

            this.currentStation= '';
            this.isPlaying = false;
            this.message= "There is a problem with the selected radio station. Please choose another station or try again later";
            this.clearTimer(this.timeOut);

        },

        playerUpdate(){

            this.playerPause();
            this.player.src = this.currentStation;
            this.player.load();

        },

        updateCurrentTime(){

            setInterval(()=>{
                let time = this.player.currentTime;
                let minutes = Math.floor(time / 60);   
                let seconds = Math.floor(time);
                seconds = (seconds - (minutes * 60 )) < 10 ? ('0' + (seconds - (minutes * 60 ))) : (seconds - (minutes * 60 )); 
                let cTime = minutes + ':' + seconds;

               this.currentTime=cTime;
            },1000)
            
        },

        playerPlay(){

            if(!this.currentStation == ''){

                this.player.play();
                this.player.addEventListener('error', this.handleMediaError);
                this.isPlaying = true;
                
            }
        },
        
        playerPause(){

            this.player.pause();
            this.isPlaying = false;

        },
        
        changeStation: function(item){

        this.change = item;
        this.currentStation = this.change.url + "?cb=" + new Date().getTime();
        this.streamName = this.change.station;
        this.streamCountry = this.change.country;

            this.playerUpdate();
            this.playerPlay(); 

        this.clearTimer(this.timeOut);

        this.timeOut =  window.setTimeout(()=>{
                this.animation = 1;
            }, 8000);

        },

        animateContent(){

            this.clearTimer(this.timeOut);

            if(this.animation == 0){

                this.animation = 1;
                return;

            }
            this.animation = 0;

        },

        favorite(){

            if(!this.change.favorite){

                this.change.favorite = true;
                return;
            }
            
            this.change.favorite = false;

        },

        clearTimer(_object){

            window.clearTimeout(_object);

        },

    },
    mounted(){

        this.playerInit();

    }

}
</script>

<style scoped>

@import url('https://use.fontawesome.com/releases/v5.6.3/css/all.css');

.audio-wrapper{
    position:absolute;
    display:flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    top:0;
    left: 0;
    overflow: hidden;

    background: -webkit-linear-gradient(to right, #9D50BB, #6E48AA);
	background: linear-gradient(to right, #9D50BB, #6E48AA);
}

.player-box{
    display: flex;
    position: relative;
    flex-direction: column;
    justify-content: space-between;
    width: 100%;
    height: 40vh;
    left: 0;

    transition-duration: 0.3s;
    -webkit-transition-duration: 0.3s;
}

.player-box__favorite{
    text-align: left;
    position: absolute;
    top: 20px;
    left: 20px;
    color: #fff;
    font-size: 20px;
    cursor:pointer;
    z-index: 1;
}

.player-box__textarea{
    position: relative;
    display:flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100%;
    width: 100%;
    top:0px;
}

.station-logo.animateContent{
    margin-top:0%;
    position: relative;
    transition-duration: 0.3s
}

.station-logo>img{
    display:block;
    width:100%;
}

.player-box__title{
    color:#fff;
    text-transform: uppercase;
    font-weight: 600;
}

.player-box__title--subline{
    font-weight: 400;
    font-size: 12px;
    margin:5px;
}

.player{
    display: flex;
    align-items: center;
    position: relative;
    flex-direction: column;
    width: 100%;
    bottom: 0;
    left: 0;
    padding: 20px 0px;
    z-index: 1;
}

.player__control-button{
    position: absolute;
    top:-25px;
    background:none;
    color:#fff;
    font-weight: 600;
    width:50px;
    height:50px;
    border:2px #fff solid;
    border-radius: 45px;
    cursor: pointer;
}

.options{
    display:flex;
    position:relative;
    justify-content: space-between;
    align-items: center;
    width:90%;
}

.options__list-button{
    position: relative;
    background: none;
    border:none;
    font-size: 20px;
    color:#fff;
    cursor:pointer;
    padding: 0;
}

.stations-list{
    display:flex;
    position: relative;
    justify-content: center;
    width:100%;
    height: 60vh;
    bottom:0;
    margin-left:0%;

    transition-duration: 0.3s;
    -webkit-transition-duration: 0.3s;
    transition-delay: 0.5s;
    -webkit-transition-delay: 0.5s;
}


.stations-list.animateContent{
    height: 0vh;
    margin-left:-100%;
    
    transition-duration: 0.3s;
    transition-delay: 0s;
    -webkit-transition-duration: 0.3s;
    -webkit-transition-delay: 0s;
    
}

.player-box.animateContent{
    height: 100vh;

    transition-delay: 0.3s;
    transition-duration: 0.3s;
    -webkit-transition-duration: 0.3s;
    -webkit-transition-delay: 0.3s;
}

.player-box.animateContent  .options__list-button{
    transform: rotate(180deg);
}

.eq{
    display:block;
    position:absolute;
    width:100%;
    bottom: 0px;
    z-index: 0;
    opacity: 0.3;
}

.sign{
    color:#fff;
    font-size: 10px;
    position: absolute;
    bottom: -5px;
    width:100%;
    text-align: center;
    opacity:0.4;
}

.sign a{
    text-decoration: none;
    color:#fff;
}

</style>
