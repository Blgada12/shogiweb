<template>
  <div>
    <h1>쇼기 테스트</h1>
    <table id="board">
      <tr v-for="i in Array.from(Array(9).keys()) " v-bind:key="i">
        <td v-for="j in Array.from(Array(9).keys())" v-bind:key="j">
          <div v-bind:id="i+'-'+j" v-bind:class="nowBoard[i][j].isMy">
            {{ nowBoard[i][j].display }}
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

let nowDisplay = []

nowDisplay = firstBoard

const downPiece = (p0, p1) => {
  if (nowDisplay[p1[0]][p1[1]].display === '') {
    console.log('비어있는고임~')
  } else {
    console.log('뭔가 있는고임')
  }
}

window.onload = () => {
  const board = document.getElementById('board')

  board.addEventListener('click', (e) => {
    if (e.target.nodeName === 'DIV') {
      console.log(e.target.id.split('-'))
      downPiece(nowDisplay[8][8], e.target.id.split('-'))
    }
  })
}

export default {
  name: 'shogi',
  data: function () {
    return {
      nowBoard: nowDisplay
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
#board>tr>td>div{
  display:table-cell;
  text-align: center;
  vertical-align:middle;
  width: 50px;
  height: 50px;
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
</style>
