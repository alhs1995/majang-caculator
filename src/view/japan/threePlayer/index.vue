<template>
  <div class="calcContainer">
    <!-- 牌區 -->
    <div class="tilesList"></div>
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
  import { ref } from 'vue'

  const stageFonSelected = ref('')
  const bonCounts = ref(0)
  const lijiCounts = ref(0)
  const ziFonSelected = ref('')
  const isRon = ref(false)
  const isHoJun = ref(false)
  const isLiji = ref(false)
  const isQanGan = ref(false)

  const onDecreaseClick = (type) => {
    if(type === 'bonCounts') {
      if(bonCounts.value === 0) return
      bonCounts.value -= 1
    } else if(type === 'lijiCounts') {
      if(lijiCounts.value === 0) return
      lijiCounts.value -= 1
    }
  }
  const onIncreaseClick = (type) => {
    if(type === 'bonCounts') {
      bonCounts.value += 1
    } else if(type === 'lijiCounts') {
      if(lijiCounts.value === 3) return
      lijiCounts.value += 1
    }
  }
</script>

<style scoped src="../../../assets/style/css/calcView.css"></style>
<style scoped src="./css/index.css"></style>