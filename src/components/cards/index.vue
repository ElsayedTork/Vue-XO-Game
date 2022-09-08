<template>
  <div class="tic-game container">
    <Info
      :counter="counter"
      :player1Wins="player1Wins"
      :player2Wins="player2Wins"
    ></Info>
    <section v-if="winner == 'x' && counter <= 9" class="tic-game__message">
      <player-one></player-one>
      <btn-continue @click="handleContinue"> </btn-continue>
    </section>
    <section
      v-else-if="winner == 'o' && counter <= 9"
      class="tic-game__message"
    >
      <player-two> </player-two>
      <btn-continue @click="handleContinue"></btn-continue>
    </section>
    <section
      v-else-if="winner == null && counter >= 9"
      class="tic-game__message"
    >
      <player-drow> </player-drow>
      <btn-continue @click="handleContinue"></btn-continue>
    </section>

    <section class="tic-game__container" v-else>
      <div class="row tic-game__container__cells">
        <Card
          class="col-4"
          v-for="sqare in sqares"
          :key="sqare.id"
          @click="addNameClass(sqare.id)"
          :sqare="sqare"
        ></Card>
      </div>
    </section>
  </div>
</template>
<script>
import Card from './card.vue';
import Info from './info.vue';
import PlayerOne from './PlayerOne.vue';
import PlayerTwo from './PlayerTwo.vue';
import PlayerDrow from './PlayerTwo.vue';
import BtnContinue from './BtnContinue.vue';

export default {
  data() {
    return {
      counter: 0,
      player1Wins: 0,
      player2Wins: 0,

      sqares: [
        { id: 1, nameClass: 'ْ' },
        { id: 2, nameClass: '' },
        { id: 3, nameClass: '' },
        { id: 4, nameClass: '' },
        { id: 5, nameClass: '' },
        { id: 6, nameClass: '' },
        { id: 7, nameClass: '' },
        { id: 8, nameClass: '' },
        { id: 9, nameClass: '' },
      ],
      xoarr: [],
      player: null,
      winner: null,
    };
  },
  components: {
    Card,
    Info,
    PlayerOne,
    PlayerTwo,
    PlayerDrow,
    BtnContinue,
  },
  methods: {
    addNameClass(id) {
      this.playOnce(id);
      this.counter % 2 == 0 ? (this.player = 'x') : (this.player = 'o');
      this.checkWin(this.player);
      this.counter++;
    },
    playOnce(id) {
      this.sqares = this.sqares.map((sqare) => {
        if (sqare.id === id && this.counter % 2 === 0) {
          this.xoarr[id] = 'x';
          return { ...sqare, nameClass: 'x' };
        } else if (sqare.id === id && this.counter % 2 !== 0) {
          this.xoarr[id] = 'o';
          return { ...sqare, nameClass: 'o' };
        } else {
          return sqare;
        }
      });
    },
    checkWin(player) {
      if (
        (this.xoarr[1] == player &&
          this.xoarr[2] == player &&
          this.xoarr[3] == player) ||
        (this.xoarr[4] == player &&
          this.xoarr[5] == player &&
          this.xoarr[6] == player) ||
        (this.xoarr[7] == player &&
          this.xoarr[8] == player &&
          this.xoarr[9] == player) ||
        (this.xoarr[3] == player &&
          this.xoarr[6] == player &&
          this.xoarr[9] == player) ||
        (this.xoarr[1] == player &&
          this.xoarr[4] == player &&
          this.xoarr[7] == player) ||
        (this.xoarr[1] == player &&
          this.xoarr[5] == player &&
          this.xoarr[9] == player) ||
        (this.xoarr[3] == player &&
          this.xoarr[5] == player &&
          this.xoarr[7] == player) ||
        (this.xoarr[2] == player &&
          this.xoarr[5] == player &&
          this.xoarr[8] == player) ||
        (this.xoarr[3] == player &&
          this.xoarr[6] == player &&
          this.xoarr[9] == player)
      ) {
        setTimeout(() => (this.winner = player), 1000);
        player == 'x' ? this.player1Wins++ : this.player2Wins++;
      }
    },
    handleContinue() {
      this.counter = 0;
      this.player = null;
      this.winner = null;
      this.sqares = [
        { id: 1, nameClass: 'ْ' },
        { id: 2, nameClass: '' },
        { id: 3, nameClass: '' },
        { id: 4, nameClass: '' },
        { id: 5, nameClass: '' },
        { id: 6, nameClass: '' },
        { id: 7, nameClass: '' },
        { id: 8, nameClass: '' },
        { id: 9, nameClass: '' },
      ];
      this.xoarr = [];
    },
  },
};
</script>
<style lang="scss">
.tic-game {
  &__message {
    background-color: #000;
    color: orange;
    padding: 80px 20px;
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
