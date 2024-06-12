<template>
  <div :class="tiles.tileContainer">
    <div :class="tileClass" :tile="props.tile" @click="onTileClick()">
    </div>
    <div v-if="props.spaceType === 'show'" :class="tiles.counter">{{ props.counts }}</div>
    <div v-else-if="props.spaceType === 'hand' && props.selected && props.tile !== 'none'" :class="tiles.counter"
      @click="onDelClick()">X</div>
  </div>
</template>

<script setup>
import tiles from '@/assets/style/scss/module/japanese/tiles.module.scss'
import { defineProps, defineEmits, computed } from 'vue'
const props = defineProps({
  spaceType: String, // 类型: hand: 手牌  show: 牌堆
  tile: String,
  counts: {
    type: Number,
    default: 4
  },
  selected: {
    type: Boolean,
    default: false
  },
  href: String
})
const emits = defineEmits(['tileClick', 'delClick', 'setClick'])
// const isSelected = toRef(props, 'selected')
const tileClass = computed(() => {
  let rtnStr = tiles.singleTile
  if (props.spaceType === 'hand') {
    rtnStr += ' ' + tiles.none
  } else if (props.spaceType === 'show') {
    if (props.tile.includes('a')) rtnStr += ' ' + tiles.aka
  }
  if (props.selected) rtnStr += ' ' + tiles.selected
  return rtnStr
})
const onTileClick = () => {
  if (props.spaceType === 'show') {
    if (props.counts)
      emits('tileClick', props.tile)
  } else if (props.spaceType === 'hand') {
    if (props.tile === 'none') {
      emits('setClick', props.href)// 设定
    } else {
      emits('tileClick', props.href) //编辑
    }
  }
}
const onDelClick = () => {
  emits('delClick')
}
</script>

<style lang="scss" scoped></style>