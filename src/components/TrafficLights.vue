<template>
  <div :class="[color, { flashing: isFlashing }]" class="traffic-lights">
    <div class="traffic-lights__lamp traffic-lights__lamp--red">
      <span v-show="color === 'red'" class="timer">{{ timer }}</span>
    </div>
    <div class="traffic-lights__lamp traffic-lights__lamp--yellow">
      <span v-show="color === 'yellow'" class="timer">{{ timer }}</span>
    </div>
    <div class="traffic-lights__lamp traffic-lights__lamp--green">
      <span v-show="color === 'green'" class="timer">{{ timer }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "TrafficLights",

  data() {
    return {
      colorList: [
        {
          color: "red",
          workingTime: 10000,
          flashingTime: 3000,
        },
        {
          color: "yellow",
          workingTime: 3000,
          flashingTime: 3000,
        },
        {
          color: "green",
          workingTime: 15000,
          flashingTime: 3000,
        },
      ],
      order: 1,
      color: "",
      timer: null,
      isFlashing: false,
    };
  },

  methods: {
    setColor(color) {
      this.color = color;
      const idx = this.colorList.findIndex((item) => item.color === color);
      const colorConfig = this.colorList[idx];

      if (color === "red") this.order = 1;
      if (color === "green") this.order = -1;

      setTimeout(() => {
        this.isFlashing = false;
        const nextColor = this.colorList[idx + this.order];
        this.$router.push({
          name: "traffic-lights",
          params: { color: nextColor.color },
        });
      }, colorConfig.workingTime);

      setTimeout(() => {
        this.isFlashing = true;
      }, colorConfig.workingTime - colorConfig.flashingTime);

      this.timer = colorConfig.workingTime / 1000;
    },
    tick() {
      this.timer--;
      if (this.timer === 0) this.timer = null;
      setTimeout(() => {
        this.tick();
      }, 1000);
    },
  },

  watch: {
    $route() {
      if (!this.$route.params.color) {
        let currentColor;
        if (!localStorage.color) {
          currentColor = this.colorList[0].color;
        } else {
          currentColor = localStorage.color;
        }
        this.$router.push({
          name: "traffic-lights",
          params: { color: currentColor },
        });
      } else {
        localStorage.color = this.$route.params.color;
        this.setColor(localStorage.color);
      }
    },
  },

  mounted() {
    this.tick();
  },
};
</script>

<style lang="scss">
.traffic-lights {
  width: 35vh;
  height: 45vh;
  margin: 0 auto;
  padding-bottom: 2vh;
  position: relative;
  top: 10vh;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: center;
  gap: 2.8vh;
  background: url("../assets/traffic-lights.svg") no-repeat center;
  background-size: contain;

  &__lamp {
    width: 11vh;
    height: 11vh;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    background: url("../assets/light-turned-off.svg") no-repeat center;
    background-size: contain;
  }
}

.timer {
  font-size: 34px;

  @media (max-width: 768px) {
    font-size: 24px;
  }
}

.red > .traffic-lights__lamp--red {
  background-image: url("../assets/light-red.svg");
}

.yellow > .traffic-lights__lamp--yellow {
  background-image: url("../assets/light-yellow.svg");
}

.green > .traffic-lights__lamp--green {
  background-image: url("../assets/light-green.svg");
}

.flashing > .traffic-lights__lamp {
  animation: flash 1s linear infinite;
}

@keyframes flash {
  50% {
    background-image: url("../assets/light-turned-off.svg");
  }
}
</style>
