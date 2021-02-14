<template>
  <div id="app">
    <div class="header">
      <h1 id="heading">Front End Task</h1>
      <h3>
        A simple vue.js webpage which retrieves timezone information from
        worldtimeapi.org.
      </h3>
    </div>
    <TimeDisplay v-bind:time="timeZone" />
    <TimeZone v-on:time-location="getTimeZone" />
  </div>
</template>

<script>
import TimeZone from "./components/TimeZone";
import TimeDisplay from "./components/TimeDisplay";
import axios from "axios"
export default {
  name: "App",
  components: {
    TimeZone,
    TimeDisplay,
  },
  data(){
    return{
      timeZone: "",
    }
  },
  methods:{
    getTimeZone(location){
      axios.get("http://worldtimeapi.org/api/timezone/" + location.area + "/" + location.region)
        .then(res => this.timeZone = res.data.datetime.substring(0,19).split("T").join(" "))
          
        .catch(err => console.log(err));
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: white;
}

body {
  background-color: #748eff; 
  background-size: cover;
  background-repeat: no-repeat;
}

.header{
  background-color:#5070ff;
  color:lightblue;
  padding-bottom:10px;
}

#heading{
  Background-color:#2b52ff;
}
</style>
