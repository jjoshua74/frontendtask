<template>
  <div class="container row w-100 d-flex justify-content-center">
    <div class="col-lg-4 col-md-4 col-sm-12 col-xs-12 mt-2 mb-2">
      <select
        class="text-white bg-dark form-control select-margins"
        v-model="selectedArea"
        @change="getLocations()"
        :disabled="areas.length == 0"
      >
        <option disabled selected value="">Please select an area...</option>
        <option :key="area" v-for="area in areas">
          {{ area }}
        </option>
      </select>
    </div>

    <div class="col-lg-4 col-md-4 col-sm-12 col-xs-12 mt-2 mb-2">
      <select
        class="text-white bg-dark form-control select-margins"
        v-model="selectedLocation"
        @change="getTime()"
        :disabled="locations.length <= 1"
      >
        <option disabled selected value="">Please select a location...</option>
        <option :key="location" v-for="location in locations">
          {{ location }}
        </option>
      </select>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "TimeZone",
  data() {
    return {
      areas: [],
      locations: [],
      allLocations: [],
      selectedArea: "",
      selectedLocation: "",
    };
  },
  methods: {
    getLocations() {
      this.locations = []; //reset so as to not append to a previous search

      // reset the selectedLocation since a new area has been selected
      this.selectedLocation = "";

      this.allLocations.forEach((element) => {
        let elementSplit = element.split("/");
        //first index is the area of the current item in the allLocations array
        if (elementSplit[0] == this.selectedArea) {
          //slice to remove the area and only return locations
          this.locations.push(elementSplit.slice(1).join("/"));
        }
      });

      // If the list of locations is <= 1 then there are no locations for that area.
      // The 1 item in the array will be the area listed again.
      if (this.locations.length <= 1) {
        // call getTime here since the location <select> will stay disabled due to no locations
        this.getTime();
      } else {
        // for TimeDisplay to hide the time again when the user selects a new area
        this.$emit("remove-time");
      }
    },
    getTime() {
      const timeZoneLocation = {
        area: this.selectedArea,
        location: this.selectedLocation,
      };
      this.$emit("time-zone", timeZoneLocation);
    },
  },
  async created() {
    this.$emit("loader", true)
    try {
      const response = await axios.get("http://worldtimeapi.org/api/timezone");

      response.data.forEach((element) => {
        this.allLocations.push(element);
        element = element.split("/");

        // attempt to de-duplicate the areas more efficiently.
        // all areas are put into the array with duplicates. The array is then
        // converted to a set and then back to an array.
        this.areas.push(element[0]);

        // if (!this.areas.includes(element[0])) {
        //   //ensures no duplicate areas
        //   //also removes time zones (they are upper case with the exception of etc)
        //   this.areas.push(element[0]);
        // }
      });

      this.areas = [...new Set(this.areas)];

      this.$emit("loader", false)

    } catch (e) {
      //displays the pretty alert
      this.$swal("WorldTimeAPI.org error. Please try again later.");
    }
  },
};
</script>

<style scoped>
.select-margins {
  margin: 10px;
}
</style>