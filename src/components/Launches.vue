<template>
  <div>
    <header>
      <img
        alt="SpaceX logo"
        class="logo"
        src="../assets/spacex-logo.png"
        width="300"
      />
      <h1>Welcome to spaceX launchings program !</h1>

      <div class="next-launch-card card">
        <h3>
          Next launch on
          <span class="color-dark-green fw-bold">{{
            formatDate(nextLaunch.date_local)
          }}</span>
        </h3>
        <div class="">
          <span class="fw-bold">name: </span>
          <span>{{ nextLaunch.name }} </span>
        </div>
        <div class="">
          <span class="fw-bold">time remainging: </span>
          <span
            >{{ timeToLaunch() }}{{ dateToLaunch.Days }} days,
            {{ dateToLaunch.Hours }} hours, {{ dateToLaunch.Minutes }} minutes
            and {{ dateToLaunch.Seconds }} seconds.</span
          >
        </div>
      </div>
    </header>

    <div class="select-wrapper">
      <h1>Filter launches</h1>
      <div>
        <select @change="switchSelect($event)">
          <option v-for="option in fetchTypes" v-bind:value="option">
            {{ option }}
          </option>
        </select>
      </div>
    </div>

    <div class="launches-wrapper">
      <div v-for="launch in pastLaunches" class="card launch-card">
        <h3>{{ launch.name }}</h3>
        <p class="pb-2">{{ formatDate(launch.date_local) }}</p>

        <div>
          <span class="fw-bold">Description: </span>
          <span>{{ launch.description }}</span>
        </div>

        <p>{{ launch.details }}</p>
        <div class="center">
          <img
            v-bind:src="launch.links.patch.small"
            style="width: 150px"
            alt="patch of the launch"
          />
        </div>

        <div class="pb-2">
          <span class="fw-bold">Link to article: </span>
          <a v-bind:href="launch.links.article">{{ launch.links.article }}</a>
        </div>
        <div class="pb-2">
          <span class="fw-bold">Link to youtube webcast: </span>
          <a v-bind:href="launch.links.webcast">{{ launch.links.webcast }}</a>
          <button
            type="button"
            class="btn"
            @click="showPopUp(launch.links.youtube_id)"
          >
            Open popup
          </button>
        </div>
        <div class="pb-2">
          <span class="fw-bold">Launchpad name: </span>
          <span>{{ launch.launchPadName }}</span>
        </div>

        <div class="pb-2">
          <span class="fw-bold">Payload names: </span>
          <span>
            <ul v-for="payloadName in launch.payloadNames">
              <li>{{ payloadName }}</li>
            </ul>
          </span>
        </div>

        <div class="pb-2">
          <span class="fw-bold">Custommers: </span>
          <span>
            <ul v-for="payloadCustomer in launch.payloadCustomers">
              <ul v-for="customer in payloadCustomer">
                <li>{{ customer }}</li>
              </ul>
            </ul>
          </span>
        </div>
      </div>
    </div>
  </div>
  <PopUp
    v-show="isPopUpVisible"
    :youtube-id="currentVideoId"
    @close="closePopUp"
  />
</template>

<script>
import PopUp from "./PopUp.vue";
import axios from "axios";

export default {
  name: "Launches",
  components: {
    PopUp,
  },
  data() {
    return {
      nextLaunch: [],
      pastLaunches: [],
      dateToLaunch: {
        Days: " ",
        Hours: " ",
        Minutes: " ",
        Seconds: " ",
      },
      fetchTypes: ["All", "Successfull", "Failed"],
      selectedFetchType: "All",
      isPopUpVisible: false,
      currentVideoId: "",
    };
  },
  mounted() {
    this.fetchNextLaunch();
    this.fetchAllLaunches();
  },

  computed: {},

  methods: {
    async fetchNextLaunch() {
      await fetch("https://api.spacexdata.com/v4/launches/next")
        .then((response) => response.json())
        .then((data) => {
          this.nextLaunch = data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    async fetchAllLaunches() {
      await axios
        .post("https://api.spacexdata.com/v4/launches/query", {
          query: {
            success: true,
          },
          options: {
            sort: "date_utc",
            limit: 10,
          },
        })
        .then((data) => {
          this.pastLaunches = data.data.docs;
          for (var launch in this.pastLaunches) {
            launch = this.setLaunchPadName(this.pastLaunches[launch]);
          }
          for (var launch in this.pastLaunches) {
            launch = this.setLaunchPadName(this.pastLaunches[launch]);
          }
          for (var launch in this.pastLaunches) {
            launch = this.setPayloadNames(this.pastLaunches[launch]);
          }
          for (var launch in this.pastLaunches) {
            launch = this.setPayloadCustomers(this.pastLaunches[launch]);
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },

    async fetchSuccessfullLaunches() {
      await axios
        .post("https://api.spacexdata.com/v4/launches/query", {
          query: {
            success: true,
          },
          options: {
            sort: "date_utc",
            limit: 10,
          },
        })
        .then((data) => {
          this.pastLaunches = data.data.docs;
          for (var launch in this.pastLaunches) {
            launch = this.setLaunchPadName(this.pastLaunches[launch]);
          }
          for (var launch in this.pastLaunches) {
            launch = this.setPayloadNames(this.pastLaunches[launch]);
          }
          for (var launch in this.pastLaunches) {
            launch = this.setPayloadCustomers(this.pastLaunches[launch]);
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },

    async fetchFailedLaunches() {
      await axios
        .post("https://api.spacexdata.com/v4/launches/query", {
          query: {
            success: false,
          },
          options: {
            sort: "date_utc",
            limit: 10,
          },
        })

        .then((data) => {
          this.pastLaunches = data.data.docs;
          for (var launch in this.pastLaunches) {
            launch = this.setLaunchPadName(this.pastLaunches[launch]);
          }
          for (var launch in this.pastLaunches) {
            launch = this.setPayloadNames(this.pastLaunches[launch]);
          }
          for (var launch in this.pastLaunches) {
            launch = this.setPayloadCustomers(this.pastLaunches[launch]);
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
    // methods for getting Launchpad name
    async setLaunchPadName(launch) {
      let launchPadName = await this.getLaunchPadName(launch.launchpad);
      launch["launchPadName"] = launchPadName;
      return launch;
    },

    async getLaunchPadName(id) {
      var response = await axios.get(
        "https://api.spacexdata.com/v4/launchpads/" + id
      );
      return response.data.full_name;
    },

    // methods for getting Payloads names
    async setPayloadNames(launch) {
      launch["payloadNames"] = [];
      for (var payload in launch.payloads) {
        let payloadName = await this.getPayloadNames(launch.payloads[payload]);
        launch["payloadNames"].push(payloadName);
      }
      return launch;
    },

    async getPayloadNames(id) {
      var response = await axios.get(
        "https://api.spacexdata.com/v4/payloads/" + id
      );
      return response.data.name;
    },

    // methods for getting Payloads custommer names
    async setPayloadCustomers(launch) {
      launch["payloadCustomers"] = [];
      for (var payload in launch.payloads) {
        let payloadCustomer = await this.getPayloadCustommers(
          launch.payloads[payload]
        );
        launch["payloadCustomers"].push(payloadCustomer);
      }

      return launch;
    },

    async getPayloadCustommers(id) {
      var response = await axios.get(
        "https://api.spacexdata.com/v4/payloads/" + id
      );
      return response.data.customers;
    },

    timeToLaunch() {
      setInterval(
        function () {
          var launchDate = new Date(this.nextLaunch.date_local);
          var now = new Date();

          launchDate = launchDate.getTime();
          now = now.getTime();

          const second = 1000,
            minute = second * 60,
            hour = minute * 60,
            day = hour * 24;

          var distance = launchDate - now;

          if (distance == 0) {
            this.fetchNextLaunch();
            this.timeToLaunch();
          }

          this.dateToLaunch.Days = Math.floor(distance / day);
          this.dateToLaunch.Hours = Math.floor((distance % day) / hour);
          this.dateToLaunch.Minutes = Math.floor((distance % hour) / minute);
          this.dateToLaunch.Seconds = Math.floor((distance % minute) / second);
        }.bind(this),
        100
      );
    },

    formatDate(date) {
      return new Date(date).toLocaleString();
    },

    switchSelect(event) {
      this.selectedFetchType = event.target.value;
      if (this.selectedFetchType == "All") {
        this.fetchAllLaunches();
      }
      if (this.selectedFetchType == "Successfull") {
        this.fetchSuccessfullLaunches();
      }
      if (this.selectedFetchType == "Failed") {
        this.fetchFailedLaunches();
      } else {
        this.fetchAllLaunches();
      }
    },

    showPopUp(videoId) {
      this.currentVideoId = videoId;
      this.isPopUpVisible = true;
    },
    closePopUp() {
      this.isPopUpVisible = false;
    },
  },
};
</script>

<style scoped>
@import "../assets/style.css";
</style>
