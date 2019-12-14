<template>
  <div class="mt-50">
    <el-row>
      <el-col class="text-center">
        <el-button type="primary" @click="initGame()">
          {{clickable ? '게임 재시작' : '게임 시작'}}
        </el-button>
      </el-col>
      <el-col>
        <div class="info-text">오픈된 보드에서 번호를 선택해주세요.</div>
      </el-col>
    </el-row>
    <el-row :gutter="100" class="wrap-player">
      <!-- 1 player -->
      <el-col :span="12">
        <h3 class="text-center">1 PLAYER</h3>
        <div class="wrap-board">
          <button v-for="(number,i) in playerBoard1"
            :key="i"
            :disabled="!player1"
            :id="`p1_${i+1}`"
            @click="selectNumber($event,i,number)">
            {{playerBoard1[i]}}
          </button>
        </div>
      </el-col>
      <!-- 2 player -->
      <el-col :span="12">
        <h3 class="text-center">2 PLAYER</h3>
        <div class="wrap-board">
          <button v-for="(number,i) in playerBoard2"
            :key="i"
            :disabled="player1"
            :id="`p2_${i+1}`"
            @click="selectNumber($event,i,number)">
            {{playerBoard2[i]}}
          </button>
        </div>
      </el-col>
    </el-row>
  </div>
</template>
<script>
export default {
  data() {
    return {
      playerBoard1 : [],
      playerBoard2 : [],
      clickable : false,
      player1: true,
      binghPlayer1: [],
      binghPlayer2: [],
      bingoArray : [
        [1, 2, 3, 4, 5],
        [6, 7, 8, 9, 10],
        [11, 12, 13, 14, 15],
        [16, 17, 18, 19, 20],
        [21, 22, 23, 24, 25],
        [1, 6, 11, 16, 21],
        [2, 7, 12, 17, 22],
        [3, 8, 13, 18, 23],
        [4, 9, 14, 19, 24],
        [5, 10, 15, 20, 25],
        [1, 7, 13, 19, 25],
        [5, 9, 13, 17, 21]
      ]
    }
  },
  created() {
    this.initNumber()
  },
  methods: {
    initNumber() {
      // let no = Math.floor(Math.random() * 25) + 1;
      for(let i=0; i<25; i++) {
        this.playerBoard1[i]= i + 1
        this.playerBoard2[i]= i + 1
      }
    },
    initGame() {
      this.openClick()
      this.shuffleBoard(this.playerBoard1)
      this.shuffleBoard(this.playerBoard2)
    },
    selectNumber($event,i,n) {
      this.updateBingo($event,i,n),
      this.changePlayer()
    },
    async updateBingo($event,i,n) {
      //get id
      //get number
      let position = i + 1
      let selfNumber = n
      // console.log('player1','id:',position, 'no:',selfNumber)


      //다른 플레이어 숫자판의 number 찾기
      //다른 플레이어 숫자판의 id 찾기

      let p2Id = (this.playerBoard2.findIndex((el) => el === selfNumber)) + 1
      // let p2Id = await this.findOther(selfNumber)
      let p2El = `p2_${p2Id}`
      // let p2ElResult = this.$refs.p2El
      let p2ElResult = document.getElementById(p2El)
      // console.log(p2ElResult)
      // 다른 플레이어 아이디값의 요소 찾기
      // setTimeout(this.findOther(p2Id), 5000)
      p2ElResult.classList.add('el-button--success')

      //css class추가
      $event.target.classList.add('el-button--success')

      //다신 클릭못하게 하기
    },
    // findOther(selfNumber) {
    //   return (this.playerBoard2.findIndex((el) => el === selfNumber)) + 1
    //   // let p2El = `p2_${p2Id}`
    //   // let p2ElResult = this.$refs.p2El
    //   // console.log(p2ElResult)
    // },
    openClick() {
      this.clickable = !this.clickable
    },
    changePlayer() {
      this.player1 = !this.player1
    },
    shuffleBoard(a) {
      let j, x, i
      for (i = a.length; i; i -= 1) {
          j = Math.floor(Math.random() * i)
          x = a[i - 1]
          a[i - 1] = a[j]
          a[j] = x
      }
    }
  }
}
</script>
<style lang="css">
  h3 {
    margin: 50px 0px 15px 10%
  }
  button {
    line-height: 2;
    font-size: 14px;
    border-radius: 4px;
    background: #fff;
    cursor: pointer;
    outline: 0
  }
  .mt-50 {
    margin-top: 50px
  }
  .wrap-player {
    padding: 10px 130px
  }
  .text-center {
    text-align: center
  }
  .wrap-board {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-gap: 5px;
    /* grid-auto-rows: minmax(100px, auto); */
  }
  .info-text {
    text-align: center;
    color: #999;
    width: 60%;
    margin: 10px auto;
    border-radius: 5px;
    line-height: 2;
  }
  .el-button--success {
    cursor:not-allowed
  }
</style>