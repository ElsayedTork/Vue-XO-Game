<template>
  <div class="tic-game container">
    <section v-if="playingWith == ''">
      <option-play @play-with="playWith"></option-play>
    </section>
    <section v-else>
      <Info
        :counter="counter"
        :player1Wins="player1Wins"
        :player2Wins="player2Wins"
      ></Info>

      <section v-if="textWin !== ''" class="tic-game__message">
        <p>{{ textWin }}</p>
      </section>

      <section
        v-else-if="winner == null && counter >= 9"
        class="tic-game__message"
      >
        <p>It's Drow</p>
      </section>

      <section class="tic-game__container" v-else>
        <div class="row tic-game__container__cells">
          <div
            class="col-4"
            v-for="(sqare, index) in sqares"
            :key="index"
            @click="playGame(index)"
          >
            {{ sqare }}
          </div>
        </div>
      </section>
      <div class="text-center mt-5">
        <button
          v-if="
            winner == 'x' || winner == 'o' || (winner == null && counter >= 9)
          "
          class="tic-game__btn-continue"
          @click="handleContinue"
        >
          Continue
        </button>
      </div>
    </section>
  </div>
</template>
<script>
import Info from './info.vue';
import optionPlay from './optionPlay.vue';
export default {
  data() {
    return {
      counter: 0,
      player1Wins: 0,
      player2Wins: 0,
      sqares: ['', '', '', '', '', '', '', '', ''],
      xoarr: [],
      player: null,
      winner: null,
      randomNum: null,
      endMatchPlayer1Wins: false,
      playingWith: '',
    };
  },
  computed: {
    textWin() {
      if (this.winner == 'x' && this.counter <= 9) {
        return 'Player 1 Wins';
      } else if (this.winner == 'o' && this.counter <= 9) {
        return 'Player 2 Wins';
      } else if (this.winner == null && this.counter >= 9) {
        return "it's Drow";
      } else {
        return '';
      }
    },
  },
  components: {
    Info,
    optionPlay,
  },
  methods: {
    playWith(optionToPlay) {
      this.playingWith = optionToPlay;
    },
    playGame(index) {
      if (this.playingWith == 'Players') {
        this.addNameClass(index);
      } else if (this.playingWith == 'Computer') {
        this.computerPlayer(index);
      }
    },
    computerPlayer(index) {
      if (this.xoarr[index] === undefined) {
        this.counter % 2 == 0 ? (this.player = 'x') : (this.player = 'o');
        this.playOnce(index);
        this.checkWin(this.player);
        this.counter++;
        //computer turn
        setTimeout(() => {
          if (this.endMatchPlayer1Wins == false) {
            this.player = this.player === 'x' ? 'o' : 'x';
            this.randomNumber();
            for (let i = 0; i < 6; i++) {
              if (
                this.xoarr[this.randomNum] == 'x' ||
                this.xoarr[this.randomNum] == 'o'
              ) {
                this.randomNumber();
              } else {
                break;
              }
            }
            this.playOnce(this.randomNum);
            this.checkWin(this.player);
            this.counter++;
          }
        }, 500);
      }
    },
    addNameClass(index) {
      if (this.xoarr[index] === undefined) {
        this.counter % 2 == 0 ? (this.player = 'x') : (this.player = 'o');
        this.playOnce(index);
        this.checkWin(this.player);
        this.counter++;
      }
    },
    playOnce(index) {
      if (this.counter % 2 === 0) {
        this.xoarr[index] = 'x';
        this.sqares[index] = 'x';
      } else if (this.counter % 2 !== 0) {
        this.xoarr[index] = 'o';
        this.sqares[index] = 'o';
      }
    },
    checkWin(player) {
      if (
        (this.xoarr[0] == player &&
          this.xoarr[1] == player &&
          this.xoarr[2] == player) ||
        (this.xoarr[3] == player &&
          this.xoarr[4] == player &&
          this.xoarr[5] == player) ||
        (this.xoarr[6] == player &&
          this.xoarr[7] == player &&
          this.xoarr[8] == player) ||
        (this.xoarr[2] == player &&
          this.xoarr[5] == player &&
          this.xoarr[8] == player) ||
        (this.xoarr[0] == player &&
          this.xoarr[3] == player &&
          this.xoarr[6] == player) ||
        (this.xoarr[0] == player &&
          this.xoarr[4] == player &&
          this.xoarr[8] == player) ||
        (this.xoarr[2] == player &&
          this.xoarr[4] == player &&
          this.xoarr[6] == player) ||
        (this.xoarr[1] == player &&
          this.xoarr[4] == player &&
          this.xoarr[7] == player) ||
        (this.xoarr[2] == player &&
          this.xoarr[5] == player &&
          this.xoarr[8] == player)
      ) {
        this.winner = player;
        player == 'x' ? this.player1Wins++ : this.player2Wins++;
      }
      if (this.winner == 'x') {
        this.endMatchPlayer1Wins = true;
      }
    },
    handleContinue() {
      this.counter = 0;
      this.player = null;
      this.winner = null;
      this.sqares = ['', '', '', '', '', '', '', '', ''];
      this.xoarr = [];
    },
    randomNumber() {
      this.randomNum = Math.floor(Math.random() * 9);
    },
  },
};
</script>
<style lang="scss">
.tic-game {
  &__message {
    background-color: #000;
    color: orange;
    padding: 40px 20px;
    text-align: center;
    font-size: 2rem;
    font-weight: 600;
    border-radius: 15px;
  }
  &__container {
    width: 500px;
    margin: 0 auto;
    background-color: orange;

    &__cells {
      div {
        padding: 25px 10px;
        height: 150px;
        border: 1px solid #fff;
        display: flex;
        justify-content: center;
        align-items: center;
        color: #fff;
        font-size: 60px;
        font-weight: 600;
      }
    }
  }
  &__btn-continue {
    padding: 5px 10px;
    border-radius: 10px;
    border-color: transparent;
    background-color: orange;
    color: #000;
    &:hover {
      background-color: #000;
      color: orange;
      border: 2px solid orange;
    }
  }
}
</style>
