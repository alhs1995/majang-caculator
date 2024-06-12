<template>
  <div class="calcContainer">
    <!-- 牌區 -->
    <div class="tilesList">
      <div v-for="(tileRow, index) in tilesWithRow" :key="'handRow' + index" class="tilesRow">
        <tile v-for="tile in tileRow" :key="'hand' + tile[0]" :tile="tile[0]" :counts="tile[1].counts"
          :selected="selectedTileName === tile[0]" @tileClick="onTileClick" />
      </div>
    </div>
    <!-- 選項區 -->
    <div class="optional">
      <div class="optionalCol">
        <div class="stageFon fons">
          <div class="labelTxt">場風 : </div>
          <label class="options">
            <input v-model="stageFonSelected" type="radio" name="stageFon" value="E">
            東風
          </label>
          <label class="options">
            <input v-model="stageFonSelected" type="radio" name="stageFon" value="S">
            南風
          </label>
          <label class="options">
            <input v-model="stageFonSelected" type="radio" name="stageFon" value="W">
            西風
          </label>
        </div>
        <div class="bonCounts counterLabel">
          <div class="labelTxt">本場數 : </div>
          <div class="counter">
            <div class="minus" @click="onDecreaseClick('bonCounts')">-</div>
            {{ bonCounts }}
            <div class="add" @click="onIncreaseClick('bonCounts')">+</div>
          </div>
        </div>
        <div class="lijiCounts counterLabel">
          <div class="labelTxt">立直數 : </div>
          <div class="counter">
            <button class="minus" @click="onDecreaseClick('lijiCounts')">-</button>
            {{ lijiCounts }}
            <button class="add" @click="onIncreaseClick('lijiCounts')">+</button>
          </div>
        </div>
        <div class="flexBreaker"></div>
        <div class="ziFon fons">
          <div class="labelTxt">自風 : </div>
          <label class="options">
            <input v-model="ziFonSelected" type="radio" name="ziFon" value="E">
            東風
          </label>
          <label class="options">
            <input v-model="ziFonSelected" type="radio" name="ziFon" value="S">
            南風
          </label>
          <label class="options">
            <input v-model="ziFonSelected" type="radio" name="ziFon" value="W">
            西風
          </label>
        </div>
        <div class="isRon eligible">
          <div class="labelTxt">是否榮和 :</div>
          <input v-model="isRon" type="checkbox">
        </div>
        <div class="isHoJun eligible">
          <div class="labelTxt">是否和莊 :</div>
          <input v-model="isHoJun" type="checkbox">
        </div>
        <div class="isLiji eligible">
          <div class="labelTxt">是否立直 :</div>
          <input v-model="isLiji" type="checkbox">
        </div>
        <div class="isQanGan eligible">
          <div class="labelTxt">是否搶槓 :</div>
          <input v-model="isQanGan" type="checkbox">
        </div>
      </div>
      <div class="submitBtn">送出計算</div>
    </div>
    <!-- 手牌區 -->
    <div class="calcuation"></div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import tile from '@/components/japanese/singleTile.vue'

const tileList = ref({
  "1m": {
    counts: 4
  },
  "2m": {
    counts: 4
  },
  "3m": {
    counts: 4
  },
  "4m": {
    counts: 4
  },
  "5m": {
    counts: 3
  },
  "5ma": {
    counts: 1
  },
  "6m": {
    counts: 4
  },
  "7m": {
    counts: 4
  },
  "8m": {
    counts: 4
  },
  "9m": {
    counts: 4
  },
  "1p": {
    counts: 4
  },
  "2p": {
    counts: 4
  },
  "3p": {
    counts: 4
  },
  "4p": {
    counts: 4
  },
  "5p": {
    counts: 3
  },
  "5pa": {
    counts: 1
  },
  "6p": {
    counts: 4
  },
  "7p": {
    counts: 4
  },
  "8p": {
    counts: 4
  },
  "9p": {
    counts: 4
  },
  "1s": {
    counts: 4
  },
  "2s": {
    counts: 4
  },
  "3s": {
    counts: 4
  },
  "4s": {
    counts: 4
  },
  "5s": {
    counts: 3
  },
  "5sa": {
    counts: 1
  },
  "6s": {
    counts: 4
  },
  "7s": {
    counts: 4
  },
  "8s": {
    counts: 4
  },
  "9s": {
    counts: 4
  },
  "1f": {
    counts: 4
  },
  "2f": {
    counts: 4
  },
  "3f": {
    counts: 4
  },
  "4f": {
    counts: 4
  },
  "5f": {
    counts: 4
  },
  "6f": {
    counts: 4
  },
  "7f": {
    counts: 4
  },
})
const selectedTileName = ref('')

const tilesWithRow = computed(() => {
  const filterTiles = (filterWord) => {
    return Object.entries(tileList.value).filter(e => e[0].includes(filterWord))
  }
  return [filterTiles('m'), filterTiles('p'), filterTiles('s'), filterTiles('f')]
})
const onTileClick = (tileName) => {
  if (selectedTileName.value === tileName) selectedTileName.value = ''
  else selectedTileName.value = tileName
}

const stageFonSelected = ref('')
const bonCounts = ref(0)
const lijiCounts = ref(0)
const ziFonSelected = ref('')
const isRon = ref(false)
const isHoJun = ref(false)
const isLiji = ref(false)
const isQanGan = ref(false)

const onDecreaseClick = (type) => {
  if (type === 'bonCounts') {
    if (bonCounts.value === 0) return
    bonCounts.value -= 1
  } else if (type === 'lijiCounts') {
    if (lijiCounts.value === 0) return
    lijiCounts.value -= 1
  }
}
const onIncreaseClick = (type) => {
  if (type === 'bonCounts') {
    bonCounts.value += 1
  } else if (type === 'lijiCounts') {
    if (lijiCounts.value === 3) return
    lijiCounts.value += 1
  }
}
</script>

<style scoped src="../style/scss/index.scss"></style>
<style scoped src="../../../assets/style/css/calcView.css"></style>
<style scoped src="./css/index.css"></style>