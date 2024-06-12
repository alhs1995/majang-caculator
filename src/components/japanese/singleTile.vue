<template>
  <div :class="tiles.tileContainer">
    <div :class="tileClass" :tile="props.tile" @click="onTileClick()">
    </div>
    <div :class="tiles.counter">{{ props.counts }}</div>
  </div>
</template>

<script setup>
import tiles from '@/assets/style/scss/module/japanese/tiles.module.scss'
import { defineProps, defineEmits, computed } from 'vue'
const props = defineProps({
  tile: String,
  counts: {
    type: Number,
    default: 4
  },
  selected: {
    type: Boolean,
    default: false
  }
})
const emits = defineEmits(['tileClick'])
// const isSelected = toRef(props, 'selected')
const tileClass = computed(() => {
  let rtnStr = tiles.singleTile
  if (props.tile.includes('a')) rtnStr += ' ' + tiles.aka
  if (props.selected) rtnStr += ' ' + tiles.selected
  return rtnStr
})
const onTileClick = () => {
  if (props.counts)
    emits('tileClick', props.tile)
}
</script>

<style lang="scss" scoped></style>