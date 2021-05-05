<template>
  <div id="app">
    <h1>Guess the {{ currentWord.length }}-letter word below.</h1>
    <h3>Choose from the letters below</h3>
    <div>
      <input
        readonly
        class="word-display"
        v-for="(l, i) in currentWord.split('')"
        :key="i"
        :value="getGuessedLetter(i)"
      />
    </div>
    <div>
      <button
        :class="getLetterButtonClass(l)"
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

    getGuessedLetter(index) {
      if (this.currentGuess.includes(this.currentWord[index])) {
        return this.currentWord[index];
      }

      return "";
    },

    getLetterButtonClass(letter) {
      if (this.currentGuess.includes(letter)) return "letter-button-disabled";

      return "letter-button";
    },

    makeGuess(letter) {
      if (this.currentGuess.includes(letter)) return;
      this.currentGuess.push(letter);
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Oxyden+Mono&family=Roboto+Slab:wght@400&display=swap");

* {
  /* the "*" selector can be expensive, though, use it with caution */
  font-family: "Roboto Slab", serif;
}

body {
  background: darkslategray;
  margin: 0;
  padding: 0;
}

#app {
  text-align: center;
  background: #fff;
  color: #5b6168;
  padding: 20px;
  width: 70vw;
  margin: 50px auto;
}

@media screen and (max-width: 640px) {
  #app {
    width: 90vw;
  }
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
  font-family: "Oxyden Mono", monospace;
  color: tomato;
  display: inline-block;
  width: 2vw;
  height: 2vw;
  text-align: center;
  border: 0;
  border-bottom: 2px solid #8aa1a0;
  margin: 0px 0.3vmax;
  padding: 8px;
  font-size: 32px;
  /* border: 1px solid; */
}

.letter-button,
.letter-button-disabled {
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

.letter-button-disabled {
  background: #c0c2c1;
  color: #7e7e7e;
}
</style>
