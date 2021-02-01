<template>
  <label for="player">Player</label>
  <br />
  <input type="text" name="player" v-model="playerToAdd" />
  <button @click="addPlayer">+</button>
</template>

<script>
import { ref } from "vue";

export default {
  setup(props, ctx) {
    let playerToAdd = ref("");
    const addPlayer = function () {
      ctx.emit("add", playerToAdd.value);
      playerToAdd.value = "";
    };
    return {
      playerToAdd,
      addPlayer,
    };
  },
};
</script>

<style>
</style>