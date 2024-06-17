<template>
  <div class="calcSeneContainer">
    <div class="titleBar">
      <div class="reverseBtn" @click="onCloseClick()"></div>
      <div class="titleTxt">和牌結算</div>
      <div></div>
    </div>
    <div class="calcSene">
      <div class="allTiles">
        <div class="handTile">
          <div v-for="(handTile, ind) in handTiles" :key="'handTile' + ind" class="tile"
            :class="{ aka: handTile.includes('a') }" :tile="handTile"></div>
        </div>
      </div>
      <div class="scoreContainer">
        <div class="totalScore">
          14番 <span>40符</span>
        </div>
        <div class="scoreDetail">
          <div v-for="i in 10" :key="i" class="detailShow">
            <div class="scoreType">立直</div>
            <div class="scoreBon">1 <span>番</span></div>
          </div>
        </div>
      </div>
      <div class="tileKind">
        48000点
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const handTiles = ref(['1m', '1m', '1m', '1m', '2m', '2m', '2m', '2m', '3m', '3m', '3m', '3m', '4m', '4m', '4m', '4m', '5m', '5ma'])
const emit = defineEmits(['closeSene'])
const onCloseClick = () => {
  emit('closeSene')
}
</script>

<style lang="scss" scoped>
@font-face {
  font-family: 'mahjong-jp';
  src: url('../../assets/fonts/I.MahjongJP.otf');
}

.calcSeneContainer {
  position: absolute;
  width: 950px;
  height: 760px;
  top: -70px;
  left: 0;
  background: #fff;
  z-index: 3;
  display: flex;
  flex-direction: column;

  .calcSene {
    box-sizing: border-box;
    width: 100%;
    padding: 10px;
    flex: 1 0 690px;
    display: flex;
    flex-direction: column;
    align-items: center;

    .allTiles {
      width: 100%;
      flex: 0 0 90px;
      background: #1B9008;
      display: flex;
      column-gap: 10px;
      box-sizing: border-box;
      padding: 0 10px;

      .handTile {
        height: 100%;
        flex: 1 0 0px;
        display: flex;
        align-items: center;
        justify-content: space-between;
      }

      .hoTile {
        flex: 0 0 60px;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: space-around;
      }

      .tile {
        font-family: 'mahjong-jp';
        width: 45px;
        height: 60px;
        background: #fff;
        color: #000;
        position: relative;
        border-radius: 5px;

        &::before {
          position: absolute;
          content: attr(tile);
          top: -17px;
          left: 0;
          font-size: 69px;
        }

        &.aka {
          color: #f00;
        }
      }
    }

    .scoreContainer {
      display: flex;
      flex: 1 0 0px;
      width: 100%;
      box-sizing: border-box;
      border-bottom: 4px solid #b8b8b8;

      .totalScore {
        flex: 0 0 180px;
        height: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        font-size: 45px;
        font-weight: bold;

        span {
          font-size: 20px;
        }
      }

      .scoreDetail {
        flex: 0 0 750px;
        height: 416px;
        box-sizing: border-box;
        display: flex;
        flex-direction: column;
        flex-wrap: wrap;
        align-content: space-between;
        padding: 35px;

        .detailShow {
          width: 45%;
          flex: 0 0 55px;
          display: flex;
          align-items: center;
          justify-content: space-between;
          box-sizing: border-box;
          border-bottom: 1px solid #b8b8b8;

          .scoreType {
            font-size: 25px;
          }

          .scoreBon {
            font-size: 20px;

            span {
              font-size: 16px;
            }
          }
        }
      }
    }

    .tileKind {
      flex: 0 0 160px;
      box-sizing: border-box;
      padding-top: 30px;
      font-size: 60px;
      font-weight: bold;
      color: transparent;
      background-image: linear-gradient(to top, #ff0000, #ffa600);
      background-clip: text;
    }
  }
}
</style>