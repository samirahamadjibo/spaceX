<template>
     <header>
        <img alt="SpaceX logo" class="logo" src="../assets/spacex-logo.png" width="300" />
        <h1>Welcome to spaceX launchings program ! </h1>

        <div class="next-launch-card card">
            <h3>Next launch on <span class="color-dark-green fw-bold">{{ formatDate(nextLaunch.date_local) }}</span></h3>
            <div class="">
                <span class="fw-bold">name: </span>
                <span>{{nextLaunch.name}} </span>
            </div>
            <div class="">
                <span class="fw-bold">time remainging:  </span>
                <span>{{timeToLaunch()}}{{dateToLaunch.Days}} days, {{dateToLaunch.Minutes}} minutes and {{dateToLaunch.Seconds}} seconds.</span>
            </div>
        </div>
    
    </header>
      <!-- <LazyYoutube
        ref="youtubeLazyVideo"
        src="https://www.youtube.com/watch?v=sX1Y2JMK6g8"
        max-width="720px"
        aspect-ratio="16:9"
        thumbnail-quality="standard"
    /> -->

    <div class="launches-wrapper">
        <div v-for="launch in pastLaunches" class="card launch-card">
            <h3>{{launch.name}}</h3>
            <p class="pb-4">{{formatDate(launch.date_local)}}</p>

             <div>
                <span class="fw-bold">Description: </span>
                <span>{{launch.description}}</span>
            </div>

            <p>{{launch.details}}</p>
            <div class="center">
                <img v-bind:src="launch.links.patch.small" style="width: 150px" alt="patch of the launch">
            </div>
            
            <div>
                <span class="fw-bold">Link to article: </span>
                <span>{{launch.links.article}}</span>
            </div>
            <div>
                <span class="fw-bold">Watch the webcast video: </span>
                <button type="button" class="btn" @click="showPopUp" >here</button>
            </div>
             <PopUp v-show="isPopUpVisible" @close="closePopUp"/>
             <!-- <div>
                <span class="fw-bold">Launching site: </span>
                <span>{{getLaunchpad(launch.launchpad)}}</span> 
            </div> -->
             <div>
                <span class="fw-bold">Launching playload: </span>
                <ul v-for="payload in launch.payloads">
                    <li>{{payload}}</li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import PopUp from './PopUp.vue';
import LazyYoutube from 'vue-lazytube'

export default {
  name: 'Launches',
  components: {
      PopUp,
      LazyYoutube
    },
  data() {
    return {
      nextLaunch: [],
      dateToLaunch: { Days:"", Hours: " ", Minutes: " ", Seconds:" "},
        pastLaunches: [],
        payloads: [],

        isPopUpVisible: false,
    };
  },
  mounted() {
    this.fetchNextLaunch(),
    this.fetchAllLaunches()
  },
 
  computed: {
  },

  methods:{
    async fetchNextLaunch(){
      await fetch("https://api.spacexdata.com/v4/launches/next")
      .then(response => response.json())
      .then(data => {
          this.nextLaunch = data
      })
      .catch(error => {
        alert(error)
      })
 }, 
    async fetchAllLaunches(){
      await fetch("https://api.spacexdata.com/v4/launches")
      .then(response => response.json())
      .then(data => {
          this.pastLaunches = data.slice(0,10)
      })
      .catch(error => {
        alert(error)
      })
    },
      async fetchSuccessfullLaunches(){
      await fetch("https://api.spacexdata.com/v4/launches/query")
      .then(response => response.json())
      .then(data => {
          this.pastLaunches = data.slice(0,10)
      })
      .catch(error => {
        alert(error)
      })
    },
  
    timeToLaunch(){
        setInterval(function() {
            var launchDate = new Date(this.nextLaunch.date_local)
            var now = new Date()

            launchDate = launchDate.getTime()
            now = now.getTime()

            const second = 1000,
            minute = second * 60,
            hour = minute * 60,
            day = hour * 24;

            var distance = launchDate - now

            this.dateToLaunch.Days = Math.floor(distance / (day));
            this.dateToLaunch.Hours = Math.floor((distance % (day)) / (hour));
            this.dateToLaunch.Minutes = Math.floor((distance % (hour)) / (minute));
            this.dateToLaunch.Seconds =  Math.floor((distance % (minute)) / second);

        }.bind(this), 100)
    },

    formatDate(date){
        return new Date(date).toLocaleString()
    },
    showPopUp() {
        this.isPopUpVisible = true;
      },
      closePopUp() {
        this.isPopUpVisible = false;
      },
    // getLaunchpad(id){
    //     var launchpad
    //     var link = "https://api.spacexdata.com/v4/launchpads/" + id
    //     console.log(link)
    //      fetch(link)
    //   .then(response => response.json())
    //   .then(data => {
    //       launchpad = data
    //   })
    //   .catch(error => {
    //     console.log(error)
    //   })
    //     return launchpad.name
    // }
  }
}
</script>



<style scoped>
@import '../assets/base.css';
@import '../assets/style.css';
</style>

