<template>
  <div id="app">
    <h1>Guess the {{ currentWord.length }}-letter word below.</h1>
    <h3>Choose from the letters below</h3>
    <div>
      <div class="word-display" v-for="(l, i) in currentWord.split('')" :key="i">
        {{ getGuessedLetter(i) }}
      </div>
    </div>
    <div>
      <button
        class="letter-button"
        v-for="l in alphabets"
        :key="l"
        :title="`Pick ${l}`"
        @click="makeGuess(l)"
      >
        {{ l }}
      </button>
    </div>
    <div>{{ currentWord }}</div>
    <div><button @click="loadGame">New Game</button></div>
  </div>
</template>

<script>
import Constants from "./Constants";

export default {
  name: "WordGame",

  data() {
    return {
      currentWord: "",
      currentGuess: [],
    };
  },

  mounted() {
    this.loadGame();
  },

  computed: {
    words() {
      return Constants.WORD_LIST.split(",");
    },

    alphabets() {
      return Constants.ALPHABETS.split("");
    },
  },

  methods: {
    reset() {
      this.currentWord = "";
      this.currentGuess = [];
    },

    loadGame() {
      this.reset();

      let rnd = Math.floor(Math.random() * this.words.length);
      this.currentWord = this.words[rnd].toUpperCase();
    },

    getGuessedLetter(i) {
      if (this.currentGuess.includes(this.currentWord[i])) {
        return this.currentWord[i];
      }

      return " ";
    },

    makeGuess(letter) {
      this.currentGuess.push(letter);
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto+Slab:wght@400&display=swap");

* {
  /* the "*" selector can be expensive, though, use it with caution */
  font-family: "Roboto Slab", serif;
}

#app {
  text-align: center;
  color: #5b6168;
  margin-top: 60px;
}

div {
  margin-bottom: 20px;
}

h1,
h2,
h3 {
  font-weight: 200;
  color: darkcyan;
}

h3 {
  color: #39aaaa;
}

.word-display {
  display: inline-block;
  width: 2vw;
  height: 2vw;
  border-bottom: 2px solid #8aa1a0;
  margin: 0px 0.3vmax;
  padding: 8px;
  font-size: 24px;
  border: 1px solid;
}

.letter-button {
  display: inline-block;
  text-align: center;
  width: 40px;
  border: 0;
  background: #339487;
  color: #fff;
  margin: 0.5vw;
  padding: 10px;
  font-size: 20px;
  cursor: pointer;
  transition: all 0.25s;
}

.letter-button:hover {
  background: #29b9b9;
  transform: scale(1.2);
  box-shadow: 2px 2px 4px #1114;
}
</style>
