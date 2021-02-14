<template>
  <div class="container">
    <img src="../assets/worldmap.png" />
    <p class="time-text" v-bind:class="{ hide: time == '' }">
      It is currently <br />
      {{ time }}
    </p>
  </div>
</template>

<script>
export default {
  name: "TimeDisplay",
  props: ["time"],
};
</script>

<style scoped>
.container {
  position: relative;
  color: black;
  font-size: 40px;
  font-weight: bold;
  margin-bottom: 10px;
  margin-top: 10px;
}

.hide {
  visibility: hidden;
}

img{
    height: 398px;
    width: 800px;
}

.time-text {
  background-color: rgba(255, 255, 255, 0.5);
  position:absolute;
  left: 0;
  top: 140px; 
  right: 0;
  bottom: 130px;
  margin:0px;
}
</style>