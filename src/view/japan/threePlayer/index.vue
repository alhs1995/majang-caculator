<template>
  <div class="calcContainer">
    <!-- 牌山區 -->
    <div class="tilesList">
      <div v-for="(tileRow, index) in tilesWithRow" :key="'showRow' + index" class="tilesRow">
        <tile v-for="tile in tileRow" :key="'show' + tile[0]" spaceType="show" :tile="tile[0]" :counts="tile[1]"
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
    <div class="calcuation">
      <div class="calcRow">
        <div class="calcTileBox">
          <div class="calcRowTitle">手牌</div>
          <tile 
            v-for="hand in inHandCount" 
            :key="'hand'+(hand-1)" 
            spaceType="hand" 
            :tile="showHand[hand-1]" 
            :selected="(showHand[hand-1] === 'none' && !!selectedTileName) || (showHand[hand-1] !== 'none' && selectedHandName === ('hand'+(hand-1)))" 
            @setClick="setTile('hand', hand-1)"
            @editClick="editTile('hand'+(hand-1))"
            @delClick="delTile('hand', hand-1)"
          />
        </div>
        <div class="calcTileBox">
          <div class="calcRowTitle">和牌</div>
          <tile 
            spaceType="hand" 
            :tile="showHo" 
            :selected="(showHo === 'none' && !!selectedTileName) || (showHo !== 'none' && selectedHandName === 'ho')"
            @setClick="setTile('ho', 0)"
            @editClick="editTile('ho')"
            @delClick="delTile('ho', 0)"
          />
        </div>
      </div>
      <div class="calcRow">
        <div class="calcTileBox">
          <div class="calcRowTitle">鳴牌</div>
          <template v-if="true">
            <div class="calcGroup">
              <tile spaceType="hand" tile="none" :selected="!!selectedTileName" />
              <tile spaceType="hand" tile="none" :selected="!!selectedTileName" />
              <tile spaceType="hand" tile="none" :selected="!!selectedTileName" />
              <tile spaceType="hand" tile="none" :selected="!!selectedTileName" />
            </div>
          </template>
          <div class="calcAddBtn cross"></div>
          <div class="calcAddBtn add"></div>
        </div>
      </div>
      <div class="calcRow">
        <div class="calcTileBox">
          <div class="calcRowTitle">拔北</div>
          <tile spaceType="hand" tile="none" :selected="!!selectedTileName" />
          <tile v-show="false" spaceType="hand" tile="none" :selected="!!selectedTileName" />
          <tile v-show="false" spaceType="hand" tile="none" :selected="!!selectedTileName" />
          <tile v-show="false" spaceType="hand" tile="none" :selected="!!selectedTileName" />
        </div>
        <div class="calcTileBox">
          <div class="calcRowTitle">寶牌指</div>
          <tile spaceType="hand" tile="none" :selected="!!selectedTileName" />
          <tile v-show="false" spaceType="hand" tile="none" :selected="!!selectedTileName" />
          <tile v-show="false" spaceType="hand" tile="none" :selected="!!selectedTileName" />
          <tile v-show="false" spaceType="hand" tile="none" :selected="!!selectedTileName" />
          <tile v-show="false" spaceType="hand" tile="none" :selected="!!selectedTileName" />
        </div>
        <div class="calcTileBox">
          <template v-if="isLiji">
            <div class="calcRowTitle">裏寶指</div>
            <tile spaceType="hand" tile="none" :selected="!!selectedTileName" />
            <tile v-show="false" spaceType="hand" tile="none" :selected="!!selectedTileName" />
            <tile v-show="false" spaceType="hand" tile="none" :selected="!!selectedTileName" />
            <tile v-show="false" spaceType="hand" tile="none" :selected="!!selectedTileName" />
            <tile v-show="false" spaceType="hand" tile="none" :selected="!!selectedTileName" />
          </template>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onBeforeMount } from 'vue'
import tile from '@/components/japanese/singleTile.vue'

const tileList = ref({
  '1m': 4,
  // '2m':  4,
  // '3m': 4,
  // '4m': 4,
  // '5m': 3,
  // '5ma': 1,
  // '6m': 4,
  // '7m': 4,
  // '8m': 4,
  '9m': 4,
  '1p': 4,
  '2p': 4,
  '3p': 4,
  '4p': 4,
  '5p': 3,
  '5pa': 1,
  '6p': 4,
  '7p': 4,
  '8p': 4,
  '9p': 4,
  '1s': 4,
  '2s': 4,
  '3s': 4,
  '4s': 4,
  '5s': 3,
  '5sa': 1,
  '6s': 4,
  '7s': 4,
  '8s': 4,
  '9s': 4,
  '1f': 4,
  '2f': 4,
  '3f': 4,
  '4f': 4,
  '5f': 4,
  '6f': 4,
  '7f': 4,
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
const selectedHandName = ref('')
const inHandCount = ref(13)
const showHand = ref([])
const showHo = ref('none')
const setTile = (where, idx) => {
  if (!!selectedTileName.value) {
    if (where === 'hand') {
      showHand.value[idx] = selectedTileName.value
    } else if (where === 'ho') {
      showHo.value = selectedTileName.value
    }
    tileList.value[selectedTileName.value] -= 1
    selectedTileName.value = ''
  }
}
const editTile = (whereString) => {
  if(selectedHandName.value === whereString) {
    selectedHandName.value = ''
  } else {
    selectedHandName.value = whereString
  }
}
const delTile = (where, idx) => {
  if (where === 'hand') {
    tileList.value[showHand.value[idx]] += 1
    showHand.value[idx] = 'none'
  } else if (where === 'ho') {
    tileList.value[showHo.value] += 1
    showHo.value = 'none'
  }
  selectedHandName.value = ''
}

onBeforeMount(() => {
  showHand.value.length = inHandCount.value
  showHand.value.fill('none', 0)
})
</script>

<style scoped src="../style/scss/index.scss"></style>
<style scoped src="../../../assets/style/css/calcView.css"></style>
<style scoped src="./css/index.css"></style>