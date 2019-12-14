<template>
  <div class="mt-50">
    <el-row>
      <el-col class="text-center">
        <el-button @click="openClick()">게임 시작</el-button>
      </el-col>
    </el-row>
    <hr>
    <div>
      <el-button v-for="i in 25" :key="i" :disabled="clickable">
        <!-- {{initBoard[i-1]}} -->
        {{i}}
      </el-button>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      // initBoard : []
      clickable : false
    }
  },
  mounted() {
    // this.initNumber()
  },
  methods: {
    // initNumber() {
    //   // let no = Math.floor(Math.random() * 25) + 1;
    //   // for(let i=0; i<25; i++) {
    //   //   this.initBoard[i]= i + 1
    //   // }
    //   // console.log(this.initBoard)
    // }
    openClick() {
      this.clickable = !this.clickable
    }
  }
}
</script>
<style lang="css">
  hr {
    margin: 10px 0
  }
  .mt-50 {
    margin-top: 50px
  }
  .text-center {
    text-align: center
  }
</style>