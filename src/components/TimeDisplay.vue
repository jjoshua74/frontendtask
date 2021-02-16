<template>
  <div class="container justify-content-center">
    <img class="img-fluid" src="../assets/worldmap.jpg" />
    <p class="time-text text-center" v-show=" time != '' ">
      It is currently <br />
      <u class="text-info">{{ time }}</u>
    </p>
  </div>
</template>

<script>
export default {
  name: "TimeDisplay",
  props: {
    time: String 
  },
};
</script>

<style scoped>

</style>