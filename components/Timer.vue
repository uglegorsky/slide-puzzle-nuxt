<template>
  <div class="timer">Время игры: {{ timeParse }}</div>
</template>

<script>
export default {
  name: "Timer",
  props: ["spendTime"],
  computed: {
    minutes() {
      return Math.round((this.spendTime % 3600000) / 60000);
    },
    seconds() {
      return Math.round((this.spendTime % 60000) / 1000);
    },
    timeParse() {
      return (
        (this.minutes < 10 ? "0" + this.minutes : this.minutes) +
        ":" +
        (this.seconds < 10 ? "0" + this.seconds : this.seconds)
      );
    }
  }
};
</script>

<style lang="scss" scoped>
.timer {
  font-size: 2rem;
  line-height: 3rem;
}
</style>
