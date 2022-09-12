<template>
  <form @submit.prevent="$emit('play-with', playingWith)">
    <label class="text-center mb-5">Yo Want To Play With</label>
    <select
      class="form-select"
      aria-label="Default select example"
      v-model="playingWith"
    >
      <option value="Computer">With Computer</option>
      <option value="Players">Two Player</option>
    </select>
    <div class="mt-5">
      <button>confirm</button>
    </div>
  </form>
</template>
<script>
export default {
  data() {
    return {
      playingWith: '',
    };
  },
  emits: ['play-with'],
};
</script>
<style lang="scss" scoped>
form {
  background-color: #000;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  padding: 10px 30px;
  label {
    color: #fff;
    font-size: 22px;
    font-weight: 700;
  }
  button {
    padding: 10px 20px;
    background-color: orange;
    color: #fff;
    font-size: 20px;
    border-radius: 10px;
  }
}
</style>
