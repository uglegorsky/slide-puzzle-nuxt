<template>
  <div class="container">
    <game />
  </div>
</template>

<script>
import Game from "@/components/Game";

export default {
  components: {
    game: Game,
  },
};
</script>


<style lang="scss">
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
</style>
