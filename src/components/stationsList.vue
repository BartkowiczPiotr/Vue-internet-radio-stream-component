<template>
<div class="list-wrapper">
    <h1 class="list-wrapper__header" v-if="!switchList"><span class="player-box__favorite" @click="switchCurrentList()"><i class="fas fa-star fa-fw" aria-hidden="true"></i></span> Radio stations list</h1>
    <div class="list-wrapper__overflow" v-if="!switchList">
        <div class="list" v-for="item in stations" :key="item.station"
        @click="changeStation(item)">
            <p class="list__name">{{item.station}}</p>
        </div>
    </div>
    <h1 class="list-wrapper__header" v-if="switchList"><span class="player-box__favorite" @click="switchCurrentList()"><i class="fas fa-globe fa-fw" aria-hidden="true"></i></span> Favorite stations</h1>
    <div class="list-wrapper__overflow" v-if="switchList">
        <div class="list" v-for="item in filteredItems" :key="item.station"
        @click="changeStation(item)" >
            <p class="list__name">{{item.station}}</p>
        </div>
    </div>
</div>
</template>

<script>
export default {

    //Web page with internet radio stations: http://www.radiosure.com/stations/

    name:'stationsList',
    props: {

       callMethod: "",

    },
    data(){

        return{

            switchList:false,

            stations: [
                //Web page with internet radio stations: http://www.radiosure.com/stations/
                {
                    station: 'eskaGO.pl - POP - Pop',
                    country: 'Poland',
                    website: 'www.eskago.pl',
                    url: 'http://www.eskago.pl/radio/pop',
                    favorite: false,
                },
                {
                    station: 'RMF FM - MAXXX',
                    country: 'Poland',
                    website: 'http://www.rmfon.pl/',
                    url: 'http://195.150.20.242:8000/rmf_maxxx',
                    favorite: true,
                },
                {
                    station: 'RMF FM - Party',
                    country: 'Poland',
                    website: 'http://www.rmfon.pl/',
                    url: 'http://195.150.20.242:8000/rmf_party',
                    favorite: false,
                },
                {
                    station: 'RMF FM - PRL',
                    country: 'Poland',
                    website: 'https://www.rmfon.pl',
                    url: 'http://195.150.20.242:8000/rmf_prl',
                    favorite: false,
                },
                {
                    station: 'RMF FM - Szanty',
                    country: 'Poland',
                    website: 'https://www.rmfon.pl',
                    url: 'http://195.150.20.242:8000/rmf_szanty',
                    favorite: false,
                },
                {
                    station: 'RMF FM - Teen',
                    country: 'Poland',
                    website: 'https://www.rmfon.pl',
                    url: 'http://195.150.20.242:8000/rmf_groove',
                    favorite: false,
                },
                {
                    station: 'RMF FM - Muzyka Klasyczna',
                    country: 'Poland',
                    website: 'https://www.rmfon.pl',
                    url: 'http://195.150.20.242:8000/rmf_muzyka_klasyczna',
                    favorite: false,
                },
                {
                    station: 'RMF FM - Hot New',
                    country: 'Poland',
                    website: 'https://www.rmfon.pl',
                    url: 'http://195.150.20.242:8000/rmf_hot_new',
                    favorite: false,
                },
                {
                    station: 'Radio Kampus',
                    country: 'Poland',
                    website: '',
                    url: 'http://193.0.98.66:8002/',
                    favorite: false,
                },
                {
                    station: 'OPEN.FM - Radio Złote Przeboje',
                    country: 'Poland',
                    website: '',
                    url: 'http://stream.open.fm/201',
                    favorite: false,
                },
                {
                    station: 'GoHAM Radio ®',
                    country: 'USA',
                    website: 'www.GoHAMRadio.com',
                    url: 'http://listen.radionomy.com/gohamradio',
                    favorite: false,
                },
                {
                    station: 'Big R Radio - 108.1 JAMZ',
                    country: 'USA',
                    website: 'http://player.bigrradio.com/?autoplay=29',
                    url: 'http://bigrradio.cdnstream1.com/5178_128',
                    favorite: false,
                },
                {
                    station: 'iPoweRadio',
                    country: 'USA',
                    website: 'http://www.ipoweradio.com/home',
                    url: 'http://stream.radiojar.com/aha6m2eyb1duv.mp3',
                    favorite: false,
                },
            ],

        };

    },
    methods:{

        changeStation(item){
            
            this.callMethod(item);

        },

        switchCurrentList(){

            if(this.switchList){
                this.switchList = false;
                return;
            }
            this.switchList = true;

        },

    },
    computed:{

    filteredItems() {

      return this.stations.filter(item => {
         return item.favorite;
      });  
      
    }

    },
}
</script>

<style scoped>

.list-wrapper{
    width:90%;
    display:flex;
    flex-direction: column;
    padding: 20px 0px;
    box-sizing: border-box;
    overflow: hidden;
}

.list-wrapper__overflow{
    width:110%;
    left:0;
    display:flex;
    flex-direction: column;
    overflow-y: auto;
    overflow-x: hidden;
}

.list-wrapper__header{
    width:100%;
    display:flex;
    justify-content: space-between;
    align-items: center;
    color:#fff;
    border-bottom:1px rgb(207, 207, 207) solid;
    font-size: 18px;
    padding: 10px 0px;
}

.list-wrapper__header span{
    cursor:pointer;
    font-size: 16px;
}

.list{
    width: 100%;
    border-bottom:1px rgba(255, 255, 255, 0.274) solid;
    cursor: pointer;
    color:rgb(214, 214, 214);
    text-align: left;
    padding-left:10px;
    box-sizing: border-box;
}

.list:last-child{
    border: none;
}

</style>
