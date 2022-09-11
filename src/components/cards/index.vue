<template>
  <div class="tic-game container">
    <Info
      :counter="counter"
      :player1Wins="player1Wins"
      :player2Wins="player2Wins"
    ></Info>

    <section v-if="winner == 'x' && counter <= 9" class="tic-game__message">
      <end-game> Player 1 Wins </end-game>
    </section>

    <section
      v-else-if="winner == 'o' && counter <= 9"
      class="tic-game__message"
    >
      <end-game>Player 2 Wins </end-game>
    </section>
    <section
      v-else-if="winner == null && counter >= 9"
      class="tic-game__message"
    >
      <end-game> It's Drow</end-game>
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
    };
  },
  components: {
    Info,
  },
  methods: {
    addNameClass(index) {
      if (this.xoarr[index] === undefined) {
        this.counter % 2 == 0 ? (this.player = 'x') : (this.player = 'o');

        this.playOnce(index);
        console.log('this.player', this.player);
        this.checkWin(this.player);
        this.counter++;
        console.log('this.xoarr', this.xoarr);
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
      console.log(this.sqares);
      //  return sqare;
      // }
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
        console.log('win cheacked true');
        setTimeout(() => (this.winner = player), 1000);
        player == 'x' ? this.player1Wins++ : this.player2Wins++;
      } else {
        console.log('win cheacked false');
      }
    },
  },

  // handleContinue() {
  //   this.counter = 0;
  //   this.player = null;
  //   this.winner = null;
  //   this.sqares = [
  //     { id: 1, nameClass: 'ْ' },
  //     { id: 2, nameClass: '' },
  //     { id: 3, nameClass: '' },
  //     { id: 4, nameClass: '' },
  //     { id: 5, nameClass: '' },
  //     { id: 6, nameClass: '' },
  //     { id: 7, nameClass: '' },
  //     { id: 8, nameClass: '' },
  //     { id: 9, nameClass: '' },
  //   ];
  //   this.xoarr = [];
  // },
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
