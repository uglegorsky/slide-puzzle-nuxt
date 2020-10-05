<template>
  <div :class="cellClasses">
    {{ cellNumber }}
  </div>
</template>

<script>
export default {
  name: "Cell",
  props: ["cellNumber"],
  computed: {
    cellClasses() {
      const classes = ["cell"];
      this.cellNumber === 0 ? classes.push("cell__empty") : null;
      return classes;
    }
  }
};
</script>

<style lang="scss" scoped>
.cell {
  width: 11rem;
  height: 11rem;
  margin: 5px;
  display: flex;
  flex-shrink: 0;
  justify-content: center;
  align-items: center;
  line-height: 11rem;
  background-color: #000;
  color: #ffffff;
  border-radius: 1rem;
  font-family: "Ubuntu", "Roboto", sans-serif;
  font-size: 6rem;
  &__empty {
    background: none;
    font-size: 0;
  }
}
</style>
