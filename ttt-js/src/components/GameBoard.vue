<template>
  <div class="game-board">
    <game-cell class="game-cell"
      v-for="(cell, index) in cells" :key="index"
      :owner="cell.owner"
      @click="placeStone(index)"
    />
  </div>
</template>

<script>
import GameCell from './GameCell'

/** Player1を示す定数（画像ファイルの名前に使用するためSymbolにはしていません） */
const P1 = 'p1'
/** Player2を示す定数（画像ファイルの名前に使用するためSymbolにはしていません） */
const P2 = 'p2'
/** まだ石が置かれていないセル */
const BLANK = undefined
/** プレイヤー毎の置かれる石（文字） */
const STONES = { [P1]: '○', [P2]: '×' }
/** 一直線に並ぶセルの組み合わせ。8個しかないので列挙 */
const LINES = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6]
]

export default {
  components: { GameCell },
  data () {
    return {
      cells: Array(9).fill(0).map(_ => ({ owner: BLANK })), // 全てのセルをowner = BLANKで初期化
      turn: P1
    }
  },
  computed: {
    /** 勝者(P1 or P2)。未決と引き分けの場合undefined */
    winner () {
      let winnerPName
      LINES.forEach(line => {
        const ownersInLine = line.map(index => this.cells[index].owner)
        const isAllSame = ownersInLine[0] === ownersInLine[1] && ownersInLine[0] === ownersInLine[2]
        if (isAllSame && ownersInLine[0] !== BLANK) {
          winnerPName = ownersInLine[0]
        }
      })
      return winnerPName
    },
    /** ゲームの勝敗が決まるか引き分けになっていればtrue */
    isGameEnded () {
      return this.winner || this.cells.filter(cell => cell.owner === BLANK).length === 0
    }
  },
  methods: {
    /** 指定のセルに現在のプレイヤーの石を置いて、ターンを交換。
     * すでにゲームが終了している場合や石が置けないセルの場合、何もしない。
     */
    placeStone (index) {
      if (this.isGameEnded) { return }
      if (this.cells[index].owner !== BLANK) { return }
      this.cells[index].owner = this.turn
      if (this.isGameEnded) {
        // どちらかの勝ち or 引き分け
        this.$emit('gameEnd', STONES[this.winner])
      } else {
        // ターン交代
        this.turn = this.turn === P1 ? P2 : P1
        this.$emit('nextTurn', STONES[this.turn])
      }
    }
  },
  mounted () {
    // 最初のターンを通知するためにイベントを発行
    this.$emit('nextTurn', STONES[this.turn])
  }
}
</script>

<style lang="scss" scoped>
.game-board{
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  .game-cell {
    box-sizing: border-box;
    width: 33%;
    margin: 0 -1px -1px 0;
    font-size: 100px;
    text-align: bottom;
    line-height: 100%;
    &::before {
      content: '';
      padding-top: 100%;
      display: block;
    }
  }
}
</style>
