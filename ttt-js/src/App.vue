<template>
  <div id="app">
    <div class="msg">{{ gameMsg }}</div>
    <game-board
      @nextTurn="nextTurn"
      @gameEnd="gameEnd"
    />
  </div>
</template>

<script>
import GameBoard from '@/components/GameBoard'
export default {
  name: 'app',
  components: {
    GameBoard
  },
  data () {
    return {
      gameMsg: ''
    }
  },
  methods: {
    nextTurn (player) {
      this.gameMsg = `${player}の番です`
    },
    gameEnd (winner) {
      this.gameMsg = winner ? `${winner}の勝ちです` : '引き分けです'
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  .msg {
    font-size: 2em;
  }
}
</style>
