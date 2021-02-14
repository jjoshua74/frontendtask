<template>
  <div class="container">

      <select class="form-select select-margins" v-model="selectedArea" v-on:change="getRegions(selectedArea)">
        <option disabled selected value="">Please select an area</option>
        <option v-bind:key="area" v-for="area in areas">
          {{ area }}
        </option>
      </select>
      
      <select class="form-select select-margins" v-model="selectedRegion" v-on:change="getTime()" v-bind:disabled=disableRegions>
        <option disabled selected value="">Please select a region</option>
        <option v-bind:key="region" v-for="region in regions">
          {{ region }}
        </option>
      </select>

  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "TimeZone",
  data() {
    return {
      areas: [],
      regions: [],
      disableRegions: true,
      selectedArea: "",
      selectedRegion: "",
    };
  },
  methods: {
    getRegions(area) {
      this.regions = []; //reset so as to not append to a previous search
      axios
        .get("http://worldtimeapi.org/api/timezone/" + area)
        .then((res) => {
          res.data.forEach((element) => {
            //used to separate the region from the area
            //cannot slice since some areas are divided by '/'
            let region = element.substring(area.length+1); 
            this.regions.push(region);
          });
          this.disableRegions = false;
        })
        //accepts an area and returns a string array of the regions within.
        .catch((err) => console.log(err));
    },
    getTime(){
      const location = {area: this.selectedArea, region: this.selectedRegion};
      console.log(location);
      this.$emit("time-location", location)
    }
  },
  created() {
    axios
      .get("http://worldtimeapi.org/api/timezone")
      .then((res) =>
        res.data.forEach((element) => {
          let area = element.split("/")[0];

          if (
            !this.areas.includes(area) &&
            area !== area.toUpperCase() &&
            area !== "Etc"
          ) {
            //ensures no duplicate areas
            //also removes time zones (they are upper case with the exception of etc)
            this.areas.push(area);
          }
        })
      )
      .catch((err) => console.log(err));
  },
};
</script>

<style scoped>

  .container{
    display: flex;
  }

  .select-margins{
    margin: 10px;
  }

</style>