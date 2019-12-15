<template>
  <div class="game-cell"
    @click="$emit('click')"
    :style="{
      backgroundImage: owner ? `url('/imgs/stone_${owner}.svg')` : 'none'
    }"
  >
  </div>
</template>

<script lang="ts">
import { createComponent } from '@vue/composition-api'
export default createComponent({
  props: {
    owner: { type: String }
  }
})
</script>

<style lang="scss" scoped>
.game-cell {
  border: 1px solid gray;
}
</style>
