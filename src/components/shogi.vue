<template>
  <div>
    <h1>쇼기 테스트</h1>
    <table id="board">
      <tr v-for="i in nowBoard" v-bind:key="i">
        <td v-for="j in i" v-bind:key="j">
          <div draggable="true" v-bind:id="'b'+j.position[0]+j.position[1]" v-bind:class="j.isMy ? 'myPiece' : 'enemyPiece'">
            {{ j.display }}
          </div>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>

class Piece {
  constructor (position, isMy) {
    this.position = position
    this.isMy = isMy
  }
  pieceUp () {

  }
}
class Blank {
  constructor (position) {
    this.position = position
    this.display = ''
  }
  pieceDown (position) {
    return false
  }
}

class Gokusho extends Piece {
  constructor (position, isMy) {
    super(position, isMy)
    this.display = '玉'
  }
  move (position) {
    return false
  }
}

class Hisha extends Piece {
  constructor (position, isMy) {
    super(position, isMy)
    this.display = '飛'
  }
  move (position) {
    return false
  }
}

class Kakukyo extends Piece {
  constructor (position, isMy) {
    super(position, isMy)
    this.display = '角'
  }
  move (position) {
    return false
  }
}

class Kinsho extends Piece {
  constructor (position, isMy) {
    super(position, isMy)
    this.display = '金'
  }
  move (position) {
    return false
  }
}

class Ginsho extends Piece {
  constructor (position, isMy) {
    super(position, isMy)
    this.display = '銀'
  }
  move (position) {
    return false
  }
}

class Keima extends Piece {
  constructor (position, isMy) {
    super(position, isMy)
    this.display = '桂'
  }
  move (position) {
    return false
  }
}

class Kyosha extends Piece {
  constructor (position, isMy) {
    super(position, isMy)
    this.display = '香'
  }
  move (position) {
    return false
  }
}

class Huhyo extends Piece {
  constructor (position, isMy) {
    super(position, isMy)
    this.display = '歩'
  }
  move (position) {
    return false
  }
}

const firstBoard = [
  [
    new Kyosha([0, 0], false), new Keima([0, 1], false), new Ginsho([0, 2], false), new Kinsho([0, 3], false), new Gokusho([0, 4], false), new Kinsho([0, 5], false), new Ginsho([0, 6], false), new Keima([0, 7], false), new Kyosha([0, 8], false)
  ],
  [
    new Blank([1, 0]), new Hisha([1, 1], false), new Blank([1, 2]), new Blank([1, 3]), new Blank([1, 4]), new Blank([1, 5]), new Blank([1, 6]), new Kakukyo([1, 7], false), new Blank([1, 8])
  ],
  [
    new Huhyo([2, 0], false), new Huhyo([2, 1], false), new Huhyo([2, 2], false), new Huhyo([2, 3], false), new Huhyo([2, 4], false), new Huhyo([2, 5], false), new Huhyo([2, 6], false), new Huhyo([2, 7], false), new Huhyo([2, 8], false)
  ],
  [
    new Blank([3, 0]), new Blank([3, 1]), new Blank([3, 2]), new Blank([3, 3]), new Blank([3, 4]), new Blank([3, 5]), new Blank([3, 6]), new Blank([3, 7]), new Blank([3, 8])
  ],
  [
    new Blank([4, 0]), new Blank([4, 1]), new Blank([4, 2]), new Blank([4, 3]), new Blank([4, 4]), new Blank([4, 5]), new Blank([4, 6]), new Blank([4, 7]), new Blank([4, 8])
  ],
  [
    new Blank([5, 0]), new Blank([5, 1]), new Blank([5, 2]), new Blank([5, 3]), new Blank([5, 4]), new Blank([5, 5]), new Blank([5, 6]), new Blank([5, 7]), new Blank([5, 8])
  ],
  [
    new Huhyo([6, 0], true), new Huhyo([6, 1], true), new Huhyo([6, 2], true), new Huhyo([6, 3], true), new Huhyo([6, 4], true), new Huhyo([6, 5], true), new Huhyo([6, 6], true), new Huhyo([6, 7], true), new Huhyo([6, 8], true)
  ],
  [
    new Blank([7, 0]), new Kakukyo([7, 1], true), new Blank([7, 2]), new Blank([7, 3]), new Blank([7, 4]), new Blank([7, 5]), new Blank([7, 6]), new Hisha([7, 7], true), new Blank([7, 8])
  ],
  [
    new Kyosha([8, 0], true), new Keima([8, 1], true), new Ginsho([8, 2], true), new Kinsho([8, 3], true), new Gokusho([8, 4], true), new Kinsho([8, 5], true), new Ginsho([8, 6], true), new Keima([8, 7], true), new Kyosha([8, 8], true)
  ]

]

let nowDisplay = []

nowDisplay = firstBoard

console.log(nowDisplay)


export default {
  name: 'shogi',
  data: function () {
    return {
      msg: 'Welcome to Shogi Web',
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
  text-align: center;
  width: 50px;
  height: 50px;
  border: 1px solid;
}
.myPiece{
  font-size: 20px;
}
.enemyPiece{
  color: red;
  font-size: 20px;
  transform: rotate(180deg);
}
</style>
