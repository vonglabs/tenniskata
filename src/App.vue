<template>
  <h2>Tennis Kata</h2>
  <add-player @add="savePlayer"></add-player>
  <li v-for="player in players" :key="player.id">
    {{ player.name }} - {{ player.score }} / {{ translateScore(player.score) }}
  </li>
  <label v-if="isDeuce"><b>Deuce !</b></label>
  <label v-if="hasAdvantage != false"
    >Advantage : <b>{{ hasAdvantage }}</b></label
  >
  <label v-if="hasWinner != false"
    >Winner : <b>{{ hasWinner }}</b></label
  >

  <!--<label v-if="isMaxPlayers">Le nombre de joueurs maximum est atteint.</label>-->
  <hr />
  <button v-if="!hasWinner" @click="gameOn">Game !</button>
  <button v-if="hasWinner" @click="gameOn">Play again !</button>
</template>

<script>
import { ref, computed } from "vue";
import AddPlayer from "./components/AddPlayer.vue";

export default {
  name: "App",
  components: {
    AddPlayer,
  },
  setup() {
    let players = ref([]);

    const savePlayer = function (data) {
      if (isMaxPlayers.value) return;
      if (data == "") return;
      players.value = [
        ...players.value,
        { name: data, id: Date.now(), score: 0, adv: false, win: false },
      ];
    };

    const isMaxPlayers = computed(() => {
      if (players.value.length >= 2) return true;
      else return false;
    });

    const isDeuce = computed(() => {
      if (
        players.value.length > 0 &&
        players.value[0].score >= 3 &&
        players.value[0].score == players.value[1].score
      ) {
        return true;
      } else return false;
    });

    const hasAdvantage = computed(() => {
      if (
        players.value.length == 2 &&
        players.value[0].score >= 4 &&
        players.value[0].score == players.value[1].score + 1
      ) {
        return players.value[0].name;
      }
      if (
        players.value.length == 2 &&
        players.value[1].score >= 4 &&
        players.value[1].score == players.value[0].score + 1
      ) {
        return players.value[1].name;
      } else return false;
    });

    const hasWinner = computed(() => {
      if (
        players.value.length >= 2 &&
        players.value[0].score >= 4 &&
        players.value[0].score >= players.value[1].score + 2
      ) {
        return players.value[0].name;
      } else if (
        players.value.length >= 2 &&
        players.value[1].score >= 4 &&
        players.value[1].score >= players.value[0].score + 2
      ) {
        return players.value[1].name;
      } else return false;
    });

    const gameOn = function () {
      if (hasAdvantage.value) {
        players.value[0].adv = players.value[1].adv = false;
      }
      if (hasWinner.value) {
        players.value[0].score = players.value[1].score = 0;
      } else if (players.value.length == 2) {
        Math.random() < 0.5
          ? players.value[0].score++
          : players.value[1].score++;
      } else return;
    };

    const translateScore = function (score) {
      if (score == 0) return "Love (0)";
      if (score == 1) return "Fiteen (15)";
      if (score == 2) return "Thirty (30)";
      if (score == 3) return "Forty (40)";
    };

    return {
      players,
      savePlayer,
      isDeuce,
      gameOn,
      isMaxPlayers,
      translateScore,
      hasWinner,
      hasAdvantage,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
