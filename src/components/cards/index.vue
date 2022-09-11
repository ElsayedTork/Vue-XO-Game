<template>
  <div class="tic-game container">
    <Info
      :counter="counter"
      :player1Wins="player1Wins"
      :player2Wins="player2Wins"
    ></Info>

    <section v-if="winner == 'x' && counter <= 9" class="tic-game__message">
      <p>Player 1 Wins</p>
    </section>

    <section
      v-else-if="winner == 'o' && counter <= 9"
      class="tic-game__message"
    >
      <p>Player 2 Wins</p>
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
          @click="addNameClass(index)"
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
  </div>
</template>
<script>
import Info from './info.vue';
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
      playWithComputer: true,
      rundomNum: null,
    };
  },
  components: {
    Info,
  },
  methods: {
    addNameClass(index) {
      if (this.xoarr[index] === undefined) {
        this.player = 'x';
        this.playOnce(index);
        this.checkWin(this.player);
        this.counter++;
       // this.rundomNum = Math.floor(Math.random() * 10);
        // for (let i = 0; i < 5; i++) {
        //   if (this.xoarr[this.rundomNum] == undefined) {
        //     console.log('Test');
        //     this.player = 'o';
        //     setTimeout(() => this.playOnce(this.rundomNum), 100);
        //     this.checkWin(this.player);
        //     break;
        //   } else {
        //     this.rundomNum = Math.floor(Math.random() * 10);
        //   }
        // }

        // console.log('randomNumber', this.rundomNum);
        // handleComputerPlayer();
      }
    },
    handleContinue() {
      this.counter = 0;
      this.player = null;
      this.winner = null;
      this.sqares = ['', '', '', '', '', '', '', '', ''];
      this.xoarr = [];
    },

    playOnce(index) {
      if (this.counter % 2 === 0) {
        this.xoarr[index] = 'x';
        this.sqares[index] = 'x';
      } else if (this.counter % 2 !== 0) {
        this.xoarr[index] = 'o';
        this.sqares[index] = 'o';
      }
      this.counter++;
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
        setTimeout(() => (this.winner = player), 700);
        player == 'x' ? this.player1Wins++ : this.player2Wins++;
      }
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
