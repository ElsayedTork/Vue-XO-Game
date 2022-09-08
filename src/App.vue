<template>
  Hello From App
  <cards />
</template>

<script>
import cards from './components/cards/index.vue';

export default {
  data() {
    return {};
  },
  components: {
    cards,
  },
};
</script>

<style class="scoped">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

html {
  font-family: 'Roboto', sans-serif;
}
</style>
