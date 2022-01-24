<template>
  <TrafficLights />
  <div class="stick"></div>
  <div class="footing"></div>
  <div class="horizon"></div>
  <div class="asphalt"></div>
</template>

<script>
// @ is an alias to /src
import TrafficLights from "@/components/TrafficLights.vue";

export default {
  name: "App",
  components: {
    TrafficLights,
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  overflow: hidden;
}

#app {
  width: 100vw;
  height: 100vh;
  background-color: #08e8ea50;
}

.stick {
  width: 2vh;
  height: 35vh;
  margin: 0 auto;
  background-color: #000000;
}

.footing {
  width: 20vh;
  height: 2vh;
  margin: 0 auto;
  background-color: #000000;
}

.horizon {
  width: 100vw;
  height: 1vh;
  background-color: #000000;
}

.asphalt {
  width: 100vw;
  height: 17vh;
  background-color: #676767;
}
</style>
