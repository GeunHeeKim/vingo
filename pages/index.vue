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
        <div class="text-center"><b>{{pointPlayer1}} 줄</b>이 완성되었습니다.</div>
        <div class="wrap-board">
          <div class="block" v-show="!player1"></div>
          <button v-for="(number,i) in playerBoard1"
            :key="i"
            :id="`p1_${i+1}`"
            ref="number"
            @click="selectNumber($event,i,number)">
            {{playerBoard1[i]}}
          </button>
        </div>
      </el-col>
      <!-- 2 player -->
      <el-col :span="12">
        <h3 class="text-center">2 PLAYER</h3>
        <div class="text-center"><b>{{pointPlayer2}} 줄</b>이 완성되었습니다.</div>
        <div class="wrap-board">
          <div class="block" v-show="!player2"></div>
          <button v-for="(number,i) in playerBoard2"
            :key="i"
            ref="number"
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
      player1: false,
      player2: false,
      bingoPlayer1: [],
      bingoPlayer2: [],
      pointPlayer1 : 0,
      pointPlayer2: 0,
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
    //각 플레이어 보드에 1 ~ 25 번호 채우기
    initNumber() {
      for(let i=0; i<25; i++) {
        this.playerBoard1[i]= i + 1
        this.playerBoard2[i]= i + 1
      }
    },
    initGame() {
      this.startGame()
      this.shuffleBoard(this.playerBoard1)
      this.shuffleBoard(this.playerBoard2)
      this.player1 = true
      // let allBtns = (this.$el.querySelectorAll('button'))
      // allBtns.forEach(element => {
      //   element
      // });
      this.playerBoard2.forEach(e => {
        let ghgh = `p2_${e}`
        let js = document.getElementById(ghgh)
        js.disabled = false
      })
      this.playerBoard1.forEach(e => {
        let ghgh = `p1_${e}`
        let js = document.getElementById(ghgh)
        js.disabled = false
      })
      this.bingoPlayer1 = []
      this.bingoPlayer2 = []
      this.clickable = true
    },
    selectNumber($event,i,n) {
      this.markNumber($event,i,n)
      this.changePlayer()
    },
    //선택한 번호를 화면에서 녹색으로 표시
    markNumber($event,i,n) {
      let player = '1'
      let otherPlayer = '2'
      let otherId = (this.playerBoard2.findIndex((el) => el === n)) + 1
      if($event.target.id.includes('p2')) {
        player = '2'
        otherPlayer = '1'
        otherId = (this.playerBoard1.findIndex((el) => el === n)) + 1
      }
      //다른 플레이어 숫자판의 number,id ,아이디값의 요소찾기
      let otherEl = `p${otherPlayer}_${otherId}`
      let otherResult = document.getElementById(otherEl)
      $event.target.disabled = true
      otherResult.disabled = true
      this.addBingo($event, i, n, player, otherPlayer, otherId)
      // setTimeout(this.addBingo($event, i, n, player, otherPlayer, otherId), 1000)

    },
    //각 플레이어의 정답 배열에 숫자 추가하기
    addBingo($event,i,n,player,otherPlayer, otherId) {
      let locationNo = i + 1
      if(otherPlayer === '2') {
        this.bingoPlayer1.push(locationNo)
        this.bingoPlayer2.push(otherId)
      } else {
        this.bingoPlayer2.push(locationNo)
        this.bingoPlayer1.push(otherId)
      }
      this.checkBingo(player,otherPlayer)
    },
    checkBingo(player,otherPlayer) {
      if(otherPlayer === '2') {
        this.player1Check(player)
        this.player2Check(otherPlayer)
      } else {
        this.player1Check(otherPlayer)
        this.player2Check(player)
      }
    },
    player1Check(player) {
      this.pointPlayer1 = 0
      for (let i = 0; i < this.bingoArray.length; i++) {
          if (
              this.bingoArray[i].every(
                  j => this.bingoPlayer1.includes(j)
              )
          ) {
            this.pointPlayer1 += 1
            if(this.pointPlayer1 === 5) {
              if(this.pointPlayer2 === 5)  {
                alert('무승부입니다.')
              }
              alert('PLAYER 1 Win!')
            }
          }
      }
    },
    player2Check(otherPlayer) {
      this.pointPlayer2 = 0
      for (let i = 0; i < this.bingoArray.length; i++) {
          if (
              this.bingoArray[i].every(
                  j => this.bingoPlayer2.includes(j)
              )
          ) {
            this.pointPlayer2 += 1
            if(this.pointPlayer2 === 5) {
              if(this.pointPlayer1 === 5) {
                alert('무승부입니다.')
              }
              alert('PLAYER 2 Win!')
            }
          }
      }
    },
    startGame() {
      this.clickable = !this.clickable
      this.bingoPlayer1 = []
      this.bingoPlayer2 = []
      this.player1 = false
      this.player2 = false
    },
    changePlayer() {
      this.player1 = !this.player1
      this.player2 = !this.player2
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
    margin: 50px 0px 15px 4%
  }
  button {
    line-height: 4;
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
    position: relative;
    margin-top: 5px;
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-gap: 5px;
    /* grid-auto-rows: minmax(100px, auto); */
  }
  .wrap-board .block {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 4px;
    background: rgba(0,0,0,0.2)
  }
  .info-text {
    text-align: center;
    color: #999;
    width: 60%;
    margin: 10px auto;
    border-radius: 5px;
    line-height: 2;
  }
  button[disabled] {
    background-color: #67C23A;
    border-color: #67C23A;
    cursor:not-allowed;
    color: #fff
  }
</style>