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
      <div class="submitBtn" @click="onSeneOpen()">送出計算</div>
    </div>
    <!-- 手牌區 -->
    <div class="calcuation">
      <div class="calcRow">
        <div class="calcTileBox">
          <div class="calcRowTitle">手牌</div>
          <tile v-for="(hand, idx) in showHand" :key="'hand' + idx" spaceType="hand" :tile="hand"
            :selected="(hand === 'none' && !!selectedTileName) || (hand !== 'none' && selectedHandName === ('hand' + idx))"
            @setClick="setTile('hand', idx)" @editClick="editTile('hand' + idx)" @delClick="delTile('hand', idx)" />
        </div>
        <div class="calcTileBox">
          <div class="calcRowTitle">和牌</div>
          <tile spaceType="hand" :tile="showHo"
            :selected="(showHo === 'none' && !!selectedTileName) || (showHo !== 'none' && selectedHandName === 'ho')"
            @setClick="setTile('ho', 0)" @editClick="editTile('ho')" @delClick="delTile('ho', 0)" />
        </div>
      </div>
      <div class="calcRow">
        <div class="calcTileBox">
          <div class="calcRowTitle">鳴牌</div>
          <template v-if="showMei.length > 0">
            <div v-for="(mei, idx) in showMei" :key="'mei' + idx" class="calcGroup">
              <tile v-for="(meiTile, ind) in mei.data" :key="mei.type + (idx + 1) * 10 + ind" spaceType="hand"
                :tile="meiTile"
                :selected="(meiTile === 'none' && (ind ? mei.data.some(e => e === selectedTileName) : !!selectedTileName)) || (selectedHandName == (mei.type + ((idx + 1) * 10 + ind)) && meiTile !== 'none')"
                @setClick="setTile(mei.type, (idx + 1) * 10 + ind)"
                @editClick="editTile(mei.type + ((idx + 1) * 10 + ind))"
                @delClick="delTile(mei.type, (idx + 1) * 10 + ind)" />
            </div>
          </template>
          <template v-if="!onMeiBtnShow">
            <div v-if="showMei.length > 0 && isEqual(showMei[showMei.length - 1].data, ['none'])"
              class="calcAddBtn cross" @click="onDelMeiType()"></div>
            <div v-if="showMei.length < 4" class="calcAddBtn add" @click="onAddMeiClick()"></div>
          </template>
          <template v-else>
            <div class="calcAddBtn pon" @click="onAddMeiType('Pon')"></div>
            <div class="calcAddBtn selfKan" @click="onAddMeiType('AKan')"></div>
            <div class="calcAddBtn kan" @click="onAddMeiType('MKan')"></div>
            <div class="calcAddBtn cross" @click="onAddMeiClick()"></div>
          </template>
        </div>
      </div>
      <div class="calcRow">
        <div class="calcTileBox">
          <div class="calcRowTitle">拔北</div>
          <tile v-for="(pei, idx) in showPei" :key="'pei' + idx" spaceType="hand" :tile="pei"
            :selected="(pei === 'none' && selectedTileName === '4f') || (pei === '4f' && selectedHandName === ('pei' + idx))"
            @setClick="setTile('pei', idx)" @editClick="editTile('pei' + idx)" @delClick="delTile('pei', idx)" />
        </div>
        <div class="calcTileBox">
          <div class="calcRowTitle">寶牌指</div>
          <tile v-for="(dora, idx) in showDora" :key="'dora' + idx" spaceType="hand" :tile="dora"
            :selected="(dora === 'none' && !!selectedTileName) || (dora !== 'none' && selectedHandName === ('dora' + idx))"
            @setClick="setTile('dora', idx)" @editClick="editTile('dora' + idx)" @delClick="delTile('dora', idx)" />
        </div>
        <div class="calcTileBox">
          <template v-if="isLiji">
            <div class="calcRowTitle">裏寶指</div>
            <tile v-for="(wura, idx) in showWuraDora" :key="'wura' + idx" spaceType="hand" :tile="wura"
              :selected="(wura === 'none' && !!selectedTileName) || (wura !== 'none' && selectedHandName === ('wura' + idx))"
              @setClick="setTile('wura', idx)" @editClick="editTile('wura' + idx)" @delClick="delTile('wura', idx)" />
          </template>
        </div>
      </div>
    </div>
    <clacSene v-if="showSene" @closeSene="onSeneClose()" />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import tile from '@/components/japanese/singleTile.vue'
import { isEqual } from 'lodash-es'
import clacSene from '@/components/japanese/calculatedSene.vue'

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
const inHandCount = computed(() => (13 - (showMei.value.length * 3)))
const showHand = ref(['none'])
const showHo = ref('none')
const showMei = ref([])
const MeiCount = ref({
  Pon: 0,
  MKan: 0,
  AKan: 0
})
const showPei = ref(['none'])
const showDora = ref(['none'])
const showWuraDora = ref(['none'])
const setTile = (where, idx) => {
  if (!!selectedTileName.value) {
    if (where === 'hand') {
      showHand.value[idx] = selectedTileName.value
      if (showHand.value.length < inHandCount.value && !showHand.value.some(e => e === 'none')) {
        showHand.value.push('none')
      }
    } else if (where === 'ho') {
      showHo.value = selectedTileName.value
    } else if (where === 'pei') {
      showPei.value[idx] = selectedTileName.value
      if (showPei.value.length < 4 && !showPei.value.some(e => e === 'none')) {
        showPei.value.push('none')
      }
    } else if (where === 'dora') {
      showDora.value[idx] = selectedTileName.value
      if (showDora.value.length < 5 && !showDora.value.some(e => e === 'none')) {
        showDora.value.push('none')
      }
    } else if (where === 'wura') {
      showWuraDora.value[idx] = selectedTileName.value
      if (showWuraDora.value.length < 5 && !showWuraDora.value.some(e => e === 'none')) {
        showWuraDora.value.push('none')
      }
    } else if (['Pon', 'MKan', 'AKan'].includes(where)) {
      const meiInd = Math.floor(idx / 10) - 1
      const dataInd = idx % 10
      showMei.value[meiInd].data[dataInd] = selectedTileName.value
      if (showMei.value[meiInd].data.length < (where === 'Pon' ? 3 : 4) && !showMei.value[meiInd].data.some(e => e === 'none')) {
        showMei.value[meiInd].data.push('none')
      }
    }
    tileList.value[selectedTileName.value] -= 1
    selectedTileName.value = ''
  }
}
const editTile = (whereString) => {
  if (selectedHandName.value === whereString) {
    selectedHandName.value = ''
  } else {
    selectedHandName.value = whereString
  }
}
const delTile = (where, idx) => {
  if (where === 'hand') {
    tileList.value[showHand.value[idx]] += 1
    showHand.value[idx] = 'none'
    const noneIndex = showHand.value.findIndex((e, ind) => {
      if (e !== 'none') return false
      let lastArr = []
      lastArr.length = showHand.value.length - ind
      lastArr.fill('none')
      return isEqual(showHand.value.slice(ind), lastArr)
    })
    if (noneIndex != -1)
      showHand.value.splice(noneIndex, (showHand.value.length - noneIndex), 'none')
  } else if (where === 'ho') {
    tileList.value[showHo.value] += 1
    showHo.value = 'none'
  } else if (where === 'pei') {
    tileList.value[showPei.value[idx]] += 1
    showPei.value[idx] = 'none'
    const noneIndex = showPei.value.findIndex((e, ind) => {
      if (e !== 'none') return false
      let lastArr = []
      lastArr.length = showPei.value.length - ind
      lastArr.fill('none')
      return isEqual(showPei.value.slice(ind), lastArr)
    })
    if (noneIndex != -1)
      showPei.value.splice(noneIndex, (showPei.value.length - noneIndex), 'none')
  } else if (where === 'dora') {
    tileList.value[showDora.value[idx]] += 1
    showDora.value[idx] = 'none'
    const noneIndex = showDora.value.findIndex((e, ind) => {
      if (e !== 'none') return false
      let lastArr = []
      lastArr.length = showDora.value.length - ind
      lastArr.fill('none')
      return isEqual(showDora.value.slice(ind), lastArr)
    })
    if (noneIndex != -1)
      showDora.value.splice(noneIndex, (showDora.value.length - noneIndex), 'none')
  } else if (where === 'wura') {
    tileList.value[showWuraDora.value[idx]] += 1
    showWuraDora.value[idx] = 'none'
    const noneIndex = showWuraDora.value.findIndex((e, ind) => {
      if (e !== 'none') return false
      let lastArr = []
      lastArr.length = showWuraDora.value.length - ind
      lastArr.fill('none')
      return isEqual(showWuraDora.value.slice(ind), lastArr)
    })
    if (noneIndex != -1)
      showWuraDora.value.splice(noneIndex, (showWuraDora.value.length - noneIndex), 'none')
  } else if (['Pon', 'MKan', 'AKan'].includes(where)) {
    const meiInd = Math.floor(idx / 10) - 1
    const dataInd = idx % 10
    tileList.value[showMei.value[meiInd].data[dataInd]] += 1
    showMei.value[meiInd].data[dataInd] = 'none'
    const noneIndex = showMei.value[meiInd].data.findIndex((e, ind) => {
      if (e !== 'none') return false
      let lastArr = []
      lastArr.length = showMei.value[meiInd].data.length - ind
      lastArr.fill('none')
      return isEqual(showMei.value[meiInd].data.slice(ind), lastArr)
    })
    if (noneIndex != -1)
      showMei.value[meiInd].data.splice(noneIndex, (showMei.value[meiInd].data.length - noneIndex), 'none')
  }
  selectedHandName.value = ''
}

const onMeiBtnShow = ref(false)
const onAddMeiClick = () => {
  onMeiBtnShow.value = !onMeiBtnShow.value
}
const onAddMeiType = (meiType) => {
  showMei.value.push({
    type: meiType,
    data: ['none'],
    typeOrder: MeiCount.value[meiType]
  })
  MeiCount.value[meiType] += 1
  onMeiBtnShow.value = false
}
const onDelMeiType = () => {
  if (isEqual(showMei.value[showMei.value.length - 1].data, ['none'])) {
    if (showMei.value.length === 4 && !isEqual(showHand.value, ['none'])) showHand.value.push('none')
    showMei.value.pop()
  }
}

const showSene = ref(false)
const onSeneOpen = () => {
  showSene.value = true
}
const onSeneClose = () => {
  showSene.value = false
}
</script>

<style scoped src="../style/scss/index.scss"></style>
<style scoped src="../../../assets/style/css/calcView.css"></style>
<style scoped src="./css/index.css"></style>