<template>
  <div class="text-center my-5 fs-3 d-flex justify-content-between">
    <p>{{ counter % 2 == 0 ? 'Player 1 Turn' : 'Player 2 Turn' }}</p>
    <p>player 1(x) : {{ player1Wins }} ,player 2(o) : {{ player2Wins }}</p>
  </div>
</template>
<script>
export default {
  props: ['counter', 'player1Wins', 'player2Wins'],
};
</script>
