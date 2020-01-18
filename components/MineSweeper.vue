<template>
  <div class="gameBack">
    <!--
    <b-field horizontal label="Difficulty">
      <b-select placeholder="Select a difficulty" v-model="selectedDifficulty" @input="generateBoard()">
        <option v-for="diff in lDifficulty" :value="diff.title" :key="diff.title">
          {{ diff.title }}
        </option>
      </b-select>
    </b-field>
    -->
    <div class="inset topDiv">
      <div class="mineDisplay">
        <table>
          <tr>
            <td v-for="(number, iNumber) in displayMinesLeft" :key="iNumber" :class="cDispNumberClass(number)"></td>
          </tr>
        </table>
      </div>
    </div>
    <div class="padDiv"></div>
    <div class="inset">
      <table>
        <tr v-for="(row, iRow) in board" :key="iRow">
          <td v-for="(cell, iCell) in row" :key="iCell" :class="cCaseClass(iCell,iRow)" @click="caseClick(iCell,iRow)" @contextmenu.prevent="caseRightClick(iCell,iRow)"></td>
        </tr>
      </table>
    </div>
  </div>
</template>

<style scoped>
  .topDiv {
    padding-top: 8px;
    padding-bottom: 8px;
    padding-left: 14px;
    padding-right: 14px;
  }

  .padDiv {
    padding-bottom: 12px;
  }

  .inset {
    border-width: 6px;
    border-style: inset;
    border-top-color: #808080;
    border-left-color: #808080;
    border-right-color: #ffffff;
    border-bottom-color: #ffffff;
  }

  .gameBack {
    background-color: #c0c0c0;
    padding-top: 12px;
    padding-bottom: 12px;
    padding-left: 12px;
    padding-right: 12px;
  }

  .sprite {
    background-image: url("data:image/png;base64,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");
    display: inline-block;
  }

  .dispNumber {
    height: 46px;
    width: 26px;
  }

  .dispNumber0 {
    background-position: -234px -0px;
  }

  .dispNumber1 {
    background-position: -0px -0px;
  }

  .dispNumber2 {
    background-position: -26px -0px;
  }

  .dispNumber3 {
    background-position: -52px -0px;
  }

  .dispNumber4 {
    background-position: -78px -0px;
  }

  .dispNumber5 {
    background-position: -104px -0px;
  }

  .dispNumber6 {
    background-position: -130px -0px;
  }

  .dispNumber7 {
    background-position: -156px -0px;
  }

  .dispNumber8 {
    background-position: -182px -0px;
  }

  .dispNumber9 {
    background-position: -208px -0px;
  }

  .case {
    height: 32px;
    width: 32px;
  }

  .caseMine {
    background-position: -160px -98px;
  }

  .caseDeto {
    background-position: -193px -98px;
  }

  .caseCovered {
    background-position: -0px -98px !important;
  }

  .caseFlaged {
    background-position: -64px -98px !important;
  }

  .caseQuestioned {
    background-position: -96px -98px !important;
  }

  .case0 {
    background-position: -32px -98px;
  }

  .case1 {
    background-position: -0px -131px;
  }

  .case2 {
    background-position: -32px -131px;
  }

  .case3 {
    background-position: -64px -131px;
  }

  .case4 {
    background-position: -96px -131px;
  }

  .case5 {
    background-position: -128px -131px;
  }

  .case6 {
    background-position: -160px -131px;
  }

  .case7 {
    background-position: -192px -131px;
  }

  .case8 {
    background-position: -224px -131px;
  }
</style>

<script>
  export default {
    props: {
      difficulty: {
        type: String,
        required: false,
        default: "Beginner"
      }
    },
    data() {
      return {
        selectedDifficulty: this.difficulty,
        gameloose: false,
        displayMinesLeft: ["0", "0", "0"],
        sprites: "data:image/png;base64,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",
        lDifficulty: [{
            title: "Beginner",
            nbMines: 10,
            size: {
              x: 8,
              y: 8
            }
          },
          {
            title: "Intermediate",
            nbMines: 40,
            size: {
              x: 16,
              y: 16
            }
          },
          {
            title: "Expert",
            nbMines: 99,
            size: {
              x: 24,
              y: 24
            }
          },
          {
            title: "Custom",
            nbMines: 19
          }
        ],
        board: []
      };
    },
    computed: {
      cHaveWin() {
        for (var y = 0; y < this.cDifficulty.size.y; y++) {
          for (var x = 0; x < this.cDifficulty.size.y; x++) {
            if (this.board[y][x].covered && !this.board[y][x].isBomb)
              return false;
          }
        }
        return true;
      },
      cDifficulty() {
        return this.lDifficulty.filter(el => el.title == this.selectedDifficulty)[0];
      }
    },
    created() {
      this.generateBoard()
    },
    methods: {
      generateBoard() {
        var row = [];
        for (var i = 0; i < this.cDifficulty.size.x; i++) {
          row.push({
            nb: 0,
            isBomb: false,
            covered: true,
            flaged: false,
            detonated: false,
            questioned: false
          });
        }
        for (var i = 0; i < this.cDifficulty.size.y; i++) {
          var clone = JSON.parse(JSON.stringify(row));
          this.board.push(clone);
        }
        var nbPlace = this.cDifficulty.nbMines;
        for (var i = 0; i < nbPlace; i++) {
          var x = Math.floor(Math.random() * this.cDifficulty.size.x);
          var y = Math.floor(Math.random() * this.cDifficulty.size.y);
          if (this.board[y][x].isBomb == true) {
            i--;
          } else {
            this.board[y][x].isBomb = true;
            markCases(x, y, this);
          }
        }
        this.CountDisplayMinesLeft();

        function markCases(x, y, thisP) {
          var y2 = y - 1;
          for (var i = 0; i < 3; i++) {
            var x2 = x - 1;
            for (var j = 0; j < 3; j++) {
              if (isInBoudaries(x2, y2, thisP)) thisP.board[y2][x2].nb++;
              x2 = x2 + 1;
            }
            y2 = y2 + 1;
          }

          function isInBoudaries(x, y, thisP) {
            if (
              x < 0 ||
              y < 0 ||
              x >= thisP.cDifficulty.size.x ||
              y >= thisP.cDifficulty.size.y
            )
              return false;
            return true;
          }
        }
      },
      cDispNumberClass(number) {
        var style = {
          sprite: true,
          dispNumber: true
        };
        style["dispNumber" + number] = true;
        return style;
      },
      CountDisplayMinesLeft() {
        var nbFlags = 0;
        for (var y = 0; y < this.cDifficulty.size.y; y++) {
          for (var x = 0; x < this.cDifficulty.size.y; x++) {
            if (this.board[y][x].flaged) nbFlags++;
          }
        }
        nbFlags = this.cDifficulty.nbMines - nbFlags.toString();
        nbFlags = ("000" + nbFlags).slice(-3);
        this.$set(this.displayMinesLeft, 0, nbFlags[0]);
        this.$set(this.displayMinesLeft, 1, nbFlags[1]);
        this.$set(this.displayMinesLeft, 2, nbFlags[2]);
      },
      cCaseClass(x, y) {
        var style = {
          sprite: true,
          case: true
        };
        if (this.board[y][x].questioned) {
          style.caseQuestioned = true;
        } else {
          if (this.board[y][x].flaged) {
            style.caseFlaged = true;
          } else {
            if (this.board[y][x].covered) {
              style.caseCovered = true;
            } else {
              if (this.board[y][x].isBomb) {
                style.caseMine = true;
                if (this.board[y][x].detonated) {
                  style.caseDeto = true;
                }
              } else {
                for (var i = 0; i < 8; i++) {
                  if (this.board[y][x].nb == i) {
                    style["case" + i] = true;
                    break;
                  }
                }
              }
            }
          }
        }
        return style;
      },
      caseRightClick(x, y) {
        if (!this.gameloose && !this.cHaveWin) {
          if (this.board[y][x].covered == true) {
            if (this.board[y][x].flaged) {
              this.board[y][x].flaged = false;
              this.board[y][x].questioned = true;
            } else if (this.board[y][x].questioned) {
              this.board[y][x].questioned = false;
            } else {
              this.board[y][x].flaged = true;
              this.CountDisplayMinesLeft();
            }
          }
        }
      },
      caseClick(x, y) {
        if (!this.gameloose && !this.cHaveWin) {
          this.board[y][x].covered = false;
          if (this.board[y][x].isBomb == false) {
            decoverCloseOnes(x, y, this);

            function decoverCloseOnes(x, y, thisP) {
              var y2 = y - 1;
              for (var i = 0; i < 3; i++) {
                var x2 = x - 1;
                for (var j = 0; j < 3; j++) {
                  if (isInBoudaries(x2, y2, thisP)) {
                    if (
                      thisP.board[y2][x2].isBomb == false &&
                      thisP.board[y2][x2].covered == true
                    ) {
                      thisP.board[y2][x2].covered = false;
                      if (thisP.board[y2][x2].nb == 0)
                        decoverCloseOnes(x2, y2, thisP);
                    }
                  }
                  x2 = x2 + 1;
                }
                y2 = y2 + 1;
              }

              function isInBoudaries(x, y, thisP) {
                if (
                  x < 0 ||
                  y < 0 ||
                  x >= thisP.cDifficulty.size.x ||
                  y >= thisP.cDifficulty.size.y
                )
                  return false;
                return true;
              }
            }
          } else {
            this.gameloose = true;
            this.board[y][x].detonated = true;
            for (var y = 0; y < this.cDifficulty.size.y; y++) {
              for (var x = 0; x < this.cDifficulty.size.y; x++) {
                if (this.board[y][x].isBomb) this.board[y][x].covered = false;
              }
            }
          }
        }
      }
    }
  };
</script>