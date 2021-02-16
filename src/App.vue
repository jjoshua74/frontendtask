<template>
  <div id="app" class="container text-white justify-content-center">
    <div class="text-center mt-4">
      <h4>A simple vue.js webpage which retrieves</h4>
      <h4>timezone information from worldtimeapi.org.</h4>
    </div>

    <div
      class="spinner-border text-dark box"
      v-show="isLoading"
    ></div>

    <TimeZone @time-zone="getTimeZone" @remove-time="timeZone = ''" @loader="loaderFunction"/>
    <TimeDisplay :time="timeZone" />
  </div>
</template>

<script>
import TimeZone from "./components/TimeZone";
import TimeDisplay from "./components/TimeDisplay";
import axios from "axios";
export default {
  name: "App",
  components: {
    TimeZone,
    TimeDisplay,
  },
  data() {
    return {
      timeZone: "",
      isLoading: true,
    };
  },
  methods: {
    async getTimeZone(timeZone) {
      this.isLoading = true;
      try {
        const response = await axios.get(
          "http://worldtimeapi.org/api/timezone/" +
            timeZone.area +
            "/" +
            timeZone.location
        );
        this.timeZone = response.data.datetime
          .substring(0, 19)
          .split("T")
          .join(" ");
      } catch (e) {
        //displays the pretty alert
        this.$swal("WorldTimeAPI.org error. Please try again later.");
      }
      this.isLoading = false;
    },
    loaderFunction(isLoading) {
      this.isLoading = isLoading;
    },
  },
};
</script>

<style scoped>
.box {
  position: absolute;
  top: 50%;
  left: 50%;
}
</style>
