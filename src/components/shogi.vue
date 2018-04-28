<template>
  <div>
    <h1>쇼기 테스트</h1>
    <table id="enemySaves" class="saves">
      <tr>
        <td v-for="i in Array.from(Array(enemySave.length).keys())" v-bind:key="i">
          <div v-bind:id="'m-'+'i'" class="enemyPiece">
            {{ enemySave[i].display }}
          </div>
        </td>
      </tr>
    </table>
    <table id="board" v-on:click="boardClick">
      <tr v-for="i in Array.from(Array(nowBoard.length).keys()) " v-bind:key="i">
        <td v-for="j in Array.from(Array(nowBoard[0].length).keys())" v-bind:key="j">
          <div v-bind:id="i+'-'+j" v-bind:class="nowBoard[i][j].isMy">
            {{ nowBoard[i][j].display }}
          </div>
        </td>
      </tr>
    </table>
    <table id="mySaves" class="saves">
      <tr>
        <td v-for="i in Array.from(Array(mySave.length).keys())" v-bind:key="i">
          <div v-bind:id="'m-'+'i'" class="myPiece">
            {{ mySave[i].display }}
          </div>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>

class Piece {
  constructor (isMy) {
    if (isMy) {
      this.isMy = 'myPiece'
    } else {
      this.isMy = 'enemyPiece'
    }
  }
}
class Blank {
  display = ''
}
class Gokusho extends Piece {
  constructor (isMy) {
    super(isMy)
    this.display = '玉'
  }
}
class Hisha extends Piece {
  constructor (isMy) {
    super(isMy)
    this.display = '飛'
  }
}
class Kakukyo extends Piece {
  constructor (isMy) {
    super(isMy)
    this.display = '角'
  }
}
class Kinsho extends Piece {
  constructor (isMy) {
    super(isMy)
    this.display = '金'
  }
}
class Ginsho extends Piece {
  constructor (isMy) {
    super(isMy)
    this.display = '銀'
  }
}
class Keima extends Piece {
  constructor (isMy) {
    super(isMy)
    this.display = '桂'
  }
}
class Kyosha extends Piece {
  constructor (isMy) {
    super(isMy)
    this.display = '香'
  }
}
class Huhyo extends Piece {
  constructor (isMy) {
    super(isMy)
    this.display = '歩'
  }
}
const firstBoard = [
  [
    new Kyosha(false), new Keima(false), new Ginsho(false), new Kinsho(false), new Gokusho(false), new Kinsho(false), new Ginsho(false), new Keima(false), new Kyosha(false)
  ],
  [
    new Blank(), new Hisha(false), new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Kakukyo(false), new Blank()
  ],
  [
    new Huhyo(false), new Huhyo(false), new Huhyo(false), new Huhyo(false), new Huhyo(false), new Huhyo(false), new Huhyo(false), new Huhyo(false), new Huhyo(false)
  ],
  [
    new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Blank()
  ],
  [
    new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Blank()
  ],
  [
    new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Blank()
  ],
  [
    new Huhyo(true), new Huhyo(true), new Huhyo(true), new Huhyo(true), new Huhyo(true), new Huhyo(true), new Huhyo(true), new Huhyo(true), new Huhyo(true)
  ],
  [
    new Blank(), new Kakukyo(true), new Blank(), new Blank(), new Blank(), new Blank(), new Blank(), new Hisha(true), new Blank()
  ],
  [
    new Kyosha(true), new Keima(true), new Ginsho(true), new Kinsho(true), new Gokusho(true), new Kinsho(true), new Ginsho(true), new Keima(true), new Kyosha(true)
  ]
]

let pieceSelect = false
let pieceSelectPlace = []

export default {
  name: 'shogi',
  data: function () {
    return {
      nowBoard: firstBoard,
      mySave: [],
      enemySave: []
    }
  },
  methods: {
    downPiece: function (p0, p1) {
      if (this.nowBoard[p1[0]][p1[1]].display === '') {
        this.nowBoard[p1[0]].splice(p1[1], 1, this.nowBoard[p0[0]][p0[1]])
        this.nowBoard[p0[0]].splice(p0[1], 1, new Blank())
      } else {
        this.mySave.push(this.nowBoard[p1[0]][p1[1]])
        this.nowBoard[p1[0]].splice(p1[1], 1, this.nowBoard[p0[0]][p0[1]])
        this.nowBoard[p0[0]].splice(p0[1], 1, new Blank())
        console.log(this.mySave)
      }
    },
    boardClick: function (e) {
      console.log(e.target)
      if (e.target.nodeName === 'DIV') {
        if (pieceSelect) {
          if (e.target.className !== 'myPiece') {
            this.downPiece(pieceSelectPlace, e.target.id.split('-'))
            pieceSelect = false
          }
        } else {
          if (e.target.className === 'myPiece') {
            pieceSelectPlace = e.target.id.split('-')
            pieceSelect = true
          }
        }
      }
    }
  }
}

</script>

<style scoped>
#board{
  border: 1px solid;
}
#board>tr{
  border: 1px solid;
}
#board>tr>td{
  border: 1px solid;
}
table>tr>td>div{
  display:table-cell;
  text-align: center;
  vertical-align:middle;
  width: 50px;
  height: 50px;
  margin: 0px;
  padding: 0px;
}
.myPiece{
  background: url('../assets/images/piece.png') no-repeat center center;
  background-size: 40px;
  font-size: 20px;
}
.enemyPiece{
  background: url('../assets/images/piece.png') no-repeat center center;
  background-size: 40px;
  font-size: 20px;
  transform: rotate(180deg);
}
.saves{
  height: 60px;
}
</style>
