<template>
  <div class="mainWrapper" :class="{'withbar':appShowType !== 'main'}">
    <template v-if="appShowType === 'main'">
      <div class="mainTitle">麻將分數計算器</div>
      <div class="mainSubTitle">種類選擇</div>
      <div class="mainTypeBtnList">
        <div 
          v-for="mjType in Object.entries(mjDictonary)" 
          :key="mjType[0]" 
          class="mainTypeBtn" 
          @click="onChangeShowTypeClick(mjType[0])"
        >{{ mjType[1].view.showText }}</div>
      </div>
    </template>
    <template v-else>
      <div class="titleBar">
        <div class="reverseBtn" @click="onChangeShowTypeClick('main')"></div>
        <div class="titleTxt">{{ mjDictonary[appShowType].view.showText }}</div>
        <div></div>
      </div>
      <threePlayer />
    </template>
  </div>
</template>

<script setup>
import { ref, defineAsyncComponent } from 'vue'
const threePlayer = defineAsyncComponent(() => import('@/view/japan/threePlayer/index.vue'))

const appShowType = ref('main')
const mjDetail = ref({
  mahjong_type: '',
  mahjong_branch: ''
})

const mjDictonary = ref({
  JP3: {
    view: {
      showText: '日本麻將(三人)'
    },
    detail: {
      mahjong_type: 'japanese',
      mahjong_branch: 'three_player'
    }
  }
})

const onChangeShowTypeClick = (gameType) => {
  appShowType.value = gameType
  if(gameType !== 'main') {
    mjDetail.value = mjDictonary[gameType].detail  
  } else {
    mjDetail.value = {
      mahjong_type: '',
      mahjong_branch: ''
    }
  }
}
</script>

<style src="../../assets/style/css/mainView.css"></style>