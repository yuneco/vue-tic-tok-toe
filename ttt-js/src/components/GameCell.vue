<template>
  <div class="game-cell"
    @click="$emit('click')"
    :style="{
      backgroundImage: owner ? `url('/imgs/stone_${owner}.svg')` : 'none'
    }"
  >
  </div>
</template>

<script>
export default {
  props: {
    owner: { type: String }
  }
}
</script>

<style lang="scss" scoped>
.game-cell {
  border: 1px solid gray;
  background-image: url();
}
</style>
