<template>
  <div v-on:click="boardClick">
    <h1>쇼기 테스트</h1>
    <table id="enemySaves" class="saves">
      <tr>
        <td v-for="i in Array.from(Array(enemySave.length).keys())" v-bind:key="i" v-bind:id="'en'+ i">
          <div v-bind:id="'e-'+ i" class="enPiece">
            {{ enemySave[i].display }}
          </div>
        </td>
      </tr>
    </table>
    <table id="board">
      <tr v-for="i in Array.from(Array(nowBoard.length).keys()) " v-bind:key="i">
        <td v-for="j in Array.from(Array(nowBoard[0].length).keys())" v-bind:key="j" v-bind:id="'b'+i+'-'+j">
          <div v-bind:id="i+'-'+j" v-bind:class="nowBoard[i][j].upgraded?nowBoard[i][j].isMy + ' upgrade':nowBoard[i][j].isMy">
            {{ nowBoard[i][j].display }}
          </div>
        </td>
      </tr>
    </table>
    <table id="mySaves" class="saves">
      <tr>
        <td v-for="i in Array.from(Array(mySave.length).keys())" v-bind:key="i" v-bind:id="'my'+ i">
          <div v-bind:id="'m-'+ i" class="myPiece">
            {{ mySave[i].display }}
          </div>
        </td>
      </tr>
    </table>
    <div v-if="selectMod" id="upgradeSelect">
      승급 선택<br>
      <div id="leftSelect" class="myPiece select">
        {{ selectList[0] }}
      </div>
      <div id="rightSelect" class="upgrade myPiece select">
        {{ selectList[1] }}
      </div>
    </div>
  </div>
</template>

<script>

class Piece {
  upgraded = false
  chance = true
  constructor (isMy) {
    if (isMy) {
      this.isMy = 'myPiece'
    } else {
      this.isMy = 'enPiece'
    }
  }
}
class Blank {
  display = ''
}
class Gokusho extends Piece {
  constructor (isMy) {
    super(isMy)
    this.canUpgrade = false
    this.display = '玉'
    this.canGo = [[[-1, -1], [-1, 0], [-1, 1], [0, -1], [0, 1], [1, -1], [1, 0], [1, 1]], []]
  }
}
class Hisha extends Piece {
  constructor (isMy) {
    super(isMy)
    this.canUpgrade = true
    this.display = '飛'
    this.displayDown = '飛'
    this.displayUp = '龍'
    this.canGo = [[], [[1, 0], [0, 1], [0, -1], [-1, 0]]]
  }
  upgrade () {
    this.display = this.displayUp
    this.canGo = [[[1, 1], [-1, -1], [-1, 1], [1, -1]], [[1, 0], [0, 1], [0, -1], [-1, 0]]]
    this.upgraded = true
  }
  downgrade () {
    this.display = this.displayDown
    this.canGo = [[], [[1, 0], [0, 1], [0, -1], [-1, 0]]]
    this.upgraded = false
  }
}
class Kakukyo extends Piece {
  constructor (isMy) {
    super(isMy)
    this.canUpgrade = true
    this.display = '角'
    this.displayDown = '角'
    this.displayUp = '馬'
    this.canGo = [[], [[-1, -1], [-1, 1], [1, -1], [1, 1]]]
  }
  upgrade () {
    this.display = this.displayUp
    this.canGo = [[[1, 0], [-1, 0], [0, 1], [0, -1]], [[-1, -1], [-1, 1], [1, -1], [1, 1]]]
    this.upgraded = true
  }
  downgrade () {
    this.display = this.displayDown
    this.canGo = [[], [[-1, -1], [-1, 1], [1, -1], [1, 1]]]
    this.upgraded = false
  }
}
class Kinsho extends Piece {
  constructor (isMy) {
    super(isMy)
    this.canUpgrade = false
    this.display = '金'
    this.canGo = [[[-1, -1], [-1, 0], [-1, 1], [0, -1], [0, 1], [1, 0]], []]
  }
}
class Ginsho extends Piece {
  constructor (isMy) {
    super(isMy)
    this.canUpgrade = true
    this.display = '銀'
    this.displayDown = '銀'
    this.displayUp = '銀'
    this.canGo = [[[-1, -1], [-1, 0], [-1, 1], [1, -1], [1, 1]], []]
  }
  upgrade () {
    this.display = this.displayUp
    this.canGo = [[[-1, -1], [-1, 0], [-1, 1], [0, -1], [0, 1], [1, 0]], []]
    this.upgraded = true
  }
  downgrade () {
    this.display = this.displayDown
    this.canGo = [[[-1, -1], [-1, 0], [-1, 1], [1, -1], [1, 1]], []]
    this.upgraded = false
  }
}
class Keima extends Piece {
  constructor (isMy) {
    super(isMy)
    this.canUpgrade = true
    this.display = '桂'
    this.displayDown = '桂'
    this.displayUp = '桂'
    this.canGo = [[[-2, -1], [-2, 1]], []]
  }
  upgrade () {
    this.display = this.displayUp
    this.canGo = [[[-1, -1], [-1, 0], [-1, 1], [0, -1], [0, 1], [1, 0]], []]
    this.upgraded = true
  }
  downgrade () {
    this.display = this.displayDown
    this.canGo = [[[-2, -1], [-2, 1]], []]
    this.upgraded = false
  }
}
class Kyosha extends Piece {
  constructor (isMy) {
    super(isMy)
    this.canUpgrade = true
    this.display = '香'
    this.displayDown = '香'
    this.displayUp = '香'
    this.canGo = [[], [[-1, 0]]]
  }
  upgrade () {
    this.display = this.displayUp
    this.canGo = [[[-1, -1], [-1, 0], [-1, 1], [0, -1], [0, 1], [1, 0]], []]
    this.upgraded = true
  }
  downgrade () {
    this.display = this.displayDown
    this.canGo = [[], [[-1, 0]]]
    this.upgraded = false
  }
}
class Huhyo extends Piece {
  constructor (isMy) {
    super(isMy)
    this.canUpgrade = true
    this.display = '歩'
    this.displayDown = '歩'
    this.displayUp = 'と'
    this.canGo = [[[-1, 0]], []]
  }
  upgrade () {
    this.display = this.displayUp
    this.canGo = [[[-1, -1], [-1, 0], [-1, 1], [0, -1], [0, 1], [1, 0]], []]
    this.upgraded = true
  }
  downgrade () {
    this.display = this.displayDown
    this.canGo = [[[-1, 0]], []]
    this.upgraded = false
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
let savePlace

export default {
  name: 'shogi',
  data: function () {
    return {
      nowBoard: firstBoard,
      mySave: [],
      enemySave: [],
      selectMod: false,
      selectList: []
    }
  },
  methods: {
    downPiece: function (p0, p1) {
      if (p0[0] === 'm') {
        let temp = this.mySave[p0[1]]
        temp.isMy = 'myPiece'
        this.nowBoard[p1[0]].splice(p1[1], 1, temp)
        this.mySave.splice(p0[1], 1)
      } else {
        if (this.nowBoard[p1[0]][p1[1]].display === '') {
          this.nowBoard[p1[0]].splice(p1[1], 1, this.nowBoard[p0[0]][p0[1]])
          this.nowBoard[p0[0]].splice(p0[1], 1, new Blank())
        } else {
          if (this.nowBoard[p1[0]][p1[1]].upgraded){
            this.nowBoard[p1[0]][p1[1]].downgrade()
          }
          this.mySave.push(this.nowBoard[p1[0]][p1[1]])
          this.nowBoard[p1[0]].splice(p1[1], 1, this.nowBoard[p0[0]][p0[1]])
          this.nowBoard[p0[0]].splice(p0[1], 1, new Blank())
        }
        if (p1[0] < 3 && this.nowBoard[p1[0]][p1[1]].canUpgrade && this.nowBoard[p1[0]][p1[1]].chance) {
          this.selectList = []
          this.selectList.push(this.nowBoard[p1[0]][p1[1]].display)
          this.selectList.push(this.nowBoard[p1[0]][p1[1]].displayUp)
          this.selectMod = true
          document.getElementById('b' + p1[0] + '-' + p1[1]).style.backgroundColor = 'gray'
          this.nowBoard[p1[0]][p1[1]].chance = false
          savePlace = p1
        }
      }
    },
    colorRed: function (p0) {
      if (parseInt(p0[0]) < 9 && parseInt(p0[0]) >= 0 && parseInt(p0[1]) < 9 && parseInt(p0[1]) >= 0) {
        if (document.getElementById(p0[0] + '-' + p0[1]).className !== 'myPiece') {
          document.getElementById('b' + p0[0] + '-' + p0[1]).style.backgroundColor = 'red'
        }
      }
    },
    colorAll: function (p0) {
      for (let i in Array.from(Array(9).keys())) {
        for (let j in Array.from(Array(9).keys())) {
          document.getElementById('b' + i + '-' + j).style.backgroundColor = p0
        }
      }
      for (let i in this.mySave) {
        document.getElementById('my' + i).style.backgroundColor = 'white'
      }
      for (let i in this.enemySave) {
        document.getElementById('en' + i).style.backgroundColor = 'white'
      }
    },
    upPiece: function (p0) {
      let select
      this.colorAll('green')
      if (p0[0] === 'm') {
        select = document.getElementById('my' + p0[1])
        select.style.backgroundColor = 'gray'
        if (this.mySave[p0[1]].display === '歩') {
          let isH = []
          for (let i in Array.from(Array(9).keys())) {
            for (let j in Array.from(Array(9).keys())) {
              if (this.nowBoard[i][j].display === '歩' && this.nowBoard[i][j].isMy === 'myPiece') {
                isH.push(j)
              }
            }
          }
          let isRH = []
          for (let i in Array.from(Array(9).keys())) {
            let isN = true
            for (let j in isH) {
              if (isH[j] === i) {
                isN = false
                break
              }
            }
            if (isN) {
              isRH.push(i)
            }
          }
          for (let i in isRH) {
            for (let j in Array.from(Array(9).keys())) {
              if (j < 2) {
                continue
              }
              let temp = document.getElementById(j + '-' + isRH[i]).className
              if (temp.indexOf('my') !== -1) {
                continue
              }
              if (temp.indexOf('en') !== -1) {
                continue
              }
              this.colorRed([j, isRH[i]])
            }
          }
        } else {
          for (let i in Array.from(Array(9).keys())) {
            for (let j in Array.from(Array(9).keys())) {
              let temp = document.getElementById(i + '-' + j).className
              if (temp.indexOf('my') !== -1) {
                continue
              }
              if (temp.indexOf('en') !== -1) {
                continue
              }
              this.colorRed([i, j])
            }
          }
        }
      } else {
        let canMove
        select = document.getElementById('b' + p0[0] + '-' + p0[1])
        canMove = this.nowBoard[p0[0]][p0[1]].canGo
        select.style.backgroundColor = 'gray'
        for (let i in canMove[0]) {
          this.colorRed([parseInt(p0[0]) + parseInt(canMove[0][i][0]), parseInt(p0[1]) + parseInt(canMove[0][i][1])])
        }
        for (let i in canMove[1]) {
          let j = 1
          while (true) {
            let temp
            const tempPoint = [parseInt(p0[0]) + parseInt(canMove[1][i][0]) * j, parseInt(p0[1]) + parseInt(canMove[1][i][1]) * j]
            if (tempPoint[0] >= 9 || tempPoint[0] < 0 || tempPoint[1] >= 9 || tempPoint[1] < 0) {
              break
            }
            try {
              temp = document.getElementById(tempPoint[0] + '-' + tempPoint[1]).className
            } catch (e) {
              continue
            }
            if (temp.indexOf('my') !== -1) {
              break
            }
            this.colorRed(tempPoint)
            j += 1
            if (temp.indexOf('en') !== -1) {
              break
            }
          }
        }
      }
    },
    boardClick: function (e) {
      if (e.target.nodeName === 'DIV') {
        if (this.selectMod) {
          if(e.target.id === 'rightSelect') {
            this.nowBoard[savePlace[0]][savePlace[1]].upgrade()
          }
          this.colorAll('white')
          this.selectMod = false
          pieceSelect = false
        }
        else {
          const nowPlace = e.target.id.split('-')
          if (pieceSelect) {
            if (e.target.className.indexOf('my') === -1) {
              if (pieceSelectPlace[0] === 'm') {
                if (this.mySave[pieceSelectPlace[1]].display === '歩') {
                  let canMove = false
                  let isH = []
                  for (let i in Array.from(Array(9).keys())) {
                    for (let j in Array.from(Array(9).keys())) {
                      if (this.nowBoard[i][j].display === '歩' && this.nowBoard[i][j].isMy === 'myPiece') {
                        isH.push(j)
                      }
                    }
                  }
                  let isRH = []
                  for (let i in Array.from(Array(9).keys())) {
                    let isN = true
                    for (let j in isH) {
                      if (isH[j] === i) {
                        isN = false
                        break
                      }
                    }
                    if (isN) {
                      isRH.push(i)
                    }
                  }
                  for (let i in isRH) {
                    for (let j in Array.from(Array(9).keys())) {
                      if (j < 2) {
                        continue
                      }
                      let temp = document.getElementById(j + '-' + isRH[i]).className
                      if (temp.indexOf('my') !== -1) {
                        continue
                      }
                      if (temp.indexOf('en') !== -1) {
                        continue
                      }
                      if (parseInt(j) === parseInt(nowPlace[0]) && parseInt(isRH[i]) === parseInt(nowPlace[1])) {
                        canMove = true
                        break
                      }
                    }
                    if (canMove) {
                      break
                    }
                  }
                  if (canMove) {
                    this.colorAll('white')
                    this.downPiece(pieceSelectPlace, nowPlace)
                    pieceSelect = false
                  }
                } else {
                  let canMove = false
                  for (let i in Array.from(Array(9).keys())) {
                    for (let j in Array.from(Array(9).keys())) {
                      let temp = document.getElementById(i + '-' + j).className
                      if (temp.indexOf('my') !== -1) {
                        continue
                      }
                      if (temp.indexOf('en') !== -1) {
                        continue
                      }
                      if (parseInt(i) === parseInt(nowPlace[0]) && parseInt(j) === parseInt(nowPlace[1])) {
                        canMove = true
                        break
                      }
                    }
                    if (canMove) {
                      break
                    }
                  }
                  if (canMove) {
                    this.colorAll('white')
                    this.downPiece(pieceSelectPlace, nowPlace)
                    pieceSelect = false
                  }
                }
              } else {
                const canMove = this.nowBoard[pieceSelectPlace[0]][pieceSelectPlace[1]].canGo
                let canMoveThere = false
                for (let i in canMove[0]) {
                  if (parseInt(pieceSelectPlace[0]) + parseInt(canMove[0][i][0]) === parseInt(nowPlace[0]) && parseInt(pieceSelectPlace[1]) + parseInt(canMove[0][i][1]) === parseInt(nowPlace[1])) {
                    canMoveThere = true
                    break
                  }
                }
                for (let i in canMove[1]) {
                  let preCan = false
                  let j = 1
                  while (true) {
                    let temp
                    const tempPoint = [parseInt(pieceSelectPlace[0]) + parseInt(canMove[1][i][0]) * j, parseInt(pieceSelectPlace[1]) + parseInt(canMove[1][i][1]) * j]
                    if (tempPoint[0] >= 9 || tempPoint[0] < 0 || tempPoint[1] >= 9 || tempPoint[1] < 0) {
                      break
                    }
                    try {
                      temp = document.getElementById(tempPoint[0] + '-' + tempPoint[1]).className
                    } catch (e) {
                      continue
                    }
                    if (temp.indexOf('my') !== -1) {
                      break
                    }
                    if (tempPoint[0] === parseInt(nowPlace[0]) && tempPoint[1] === parseInt(nowPlace[1])) {
                      preCan = true
                      break
                    }
                    j += 1
                    if (temp.indexOf('en') !== -1) {
                      break
                    }
                  }
                  if (preCan) {
                    canMoveThere = true
                    break
                  }
                }
                if (canMoveThere) {
                  this.colorAll('white')
                  this.downPiece(pieceSelectPlace, nowPlace)
                  pieceSelect = false
                }
              }
            } else if (pieceSelectPlace[0] === nowPlace[0] && pieceSelectPlace[1] === nowPlace[1]) {
              this.colorAll('white')
              pieceSelect = false
            }
          } else {
            if (e.target.className.indexOf('my') !== -1) {
              pieceSelectPlace = nowPlace
              this.upPiece(pieceSelectPlace)
              pieceSelect = true
            }
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
.select{
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
.enPiece{
  background: url('../assets/images/piece.png') no-repeat center center;
  background-size: 40px;
  font-size: 20px;
  transform: rotate(180deg);
}
.upgrade{
  color: red;
}
.saves{
  height: 60px;
}
</style>
