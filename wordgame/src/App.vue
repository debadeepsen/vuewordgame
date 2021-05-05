<template>
  <div id="app">
    <h1>Guess the {{ currentWord.length }}-letter word below.</h1>
    <h3 style="margin-bottom: 40px">Choose from the letters below</h3>

    <!-- A circular progress indicator -->
    <div class="progress-circle">
      <div class="pc-overlay">{{ progressPercent }}%</div>
      <div
        class="pc-background"
        :style="{
          background: `conic-gradient(#ec826f ${degrees}deg, #ddd ${degrees}deg)`,
        }"
      ></div>
    </div>

    <!-- The number of times the player has guessed -->
    <div class="tries-count">{{ tries }} tries</div>

    <!-- Displaying the guessed letters -->
    <div>
      <input
        readonly
        class="word-display"
        v-for="(l, i) in currentWord.split('')"
        :key="i"
        :value="getGuessedLetter(i)"
      />
    </div>

    <!-- The alphabet buttons -->
    <div class="button-container">
      <div
        class="button-disabling-overlay"
        :style="{
          pointerEvents: puzzleSolved ? 'all' : 'none',
          background: puzzleSolved ? '#fff7' : '#0000',
        }"
      ></div>
      <button
        :class="getLetterButtonClass(l)"
        v-for="l in alphabets"
        :key="l"
        :title="currentGuess.includes(l) ? `${l} already picked` : `Pick ${l}`"
        :style="{ cursor: currentGuess.includes(l) ? 'default' : 'pointer' }"
        @click="makeGuess(l)"
      >
        {{ l }}
      </button>
    </div>
    <div>{{ message }}</div>
    <div><button id="new_game" @click="loadGame">New Game</button></div>
    <div id="credits">
      <div>
        Background Photo by
        <a
          target="_blank"
          href="https://www.pexels.com/photo/date-arrow-calendar-time-5652114/"
        >
          Visual Tag Mx from Pexels
        </a>
      </div>
      <div>
        Word list from
        <a
          target="_blank"
          href="https://www.ef.com/wwen/english-resources/english-vocabulary/top-3000-words/"
          >https://www.ef.com/wwen/english-resources/english-vocabulary/top-3000-words/</a
        >
      </div>
    </div>
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
      tries: 0,
      progress: 0,
      message: "",
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

    progressPercent() {
      return Math.round((this.progress / this.currentWord.length) * 100);
    },

    degrees() {
      return Math.round((this.progress / this.currentWord.length) * 360);
    },

    puzzleSolved() {
      return this.progress === this.currentWord.length;
    },
  },

  methods: {
    reset() {
      this.currentWord = "";
      this.currentGuess = [];
      this.tries = 0;
      this.progress = 0;
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
      this.tries++;

      // this.currentWord.split("") -> converts the string to an array
      // then the array is filtered upon which of its elements matches the letter
      // the length of the filtered array is how many characters were guessed correctly
      this.progress += this.currentWord.split("").filter((e) => e === letter).length;

      if (this.puzzleSolved) {
        // solved
        this.message = `Congratulations! You found the word ${this.currentWord} in ${this.tries} tries! Click on the button below to begin a new game.`;
      }
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
  background-image: linear-gradient(0deg, #0003, #0003),
    url("./assets/pexels-visual-tag-mx-5652114.jpg");
  background-size: cover;
  height: 100vh;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

#app {
  text-align: center;
  background: #fffe;
  color: #5b6168;
  padding: 20px;
  width: 50vw;
  margin: 50px auto;
  position: relative;
  box-shadow: 0px 0px 6px 2px #0004;
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
  color: #ec826f;
  display: inline-block;
  width: 2vw;
  height: 2vw;
  text-align: center;
  border: 0;
  border-bottom: 2px solid #8aa1a0;
  margin: 0px 0.3vmax;
  padding: 8px;
  font-size: 32px;
  outline: none;
  background-color: transparent;
}

.button-container {
  position: relative;
}

.button-disabling-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transition: all 0.18s;
}

.letter-button,
.letter-button-disabled {
  display: inline-block;
  text-align: center;
  width: 40px;
  border: 0;
  background: #0490b3;
  box-shadow: 1px 1px 3px #1116;
  color: #fff;
  margin: 0.5vw;
  padding: 10px;
  font-size: 20px;
  transition: all 0.25s;
}

.letter-button:hover {
  background: #29b9b9;
  transform: scale(1.2);
  box-shadow: 2px 2px 4px #1114;
}

.letter-button-disabled {
  background: #c0c2c188;
  color: #9995;
}

.tries-count {
  position: absolute;
  top: 20px;
  right: 20px;
  border: 50px;
  background: #f8beb455;
  color: #ec826f;
  padding: 10px;
  border-radius: 50px;
  font-size: 14px;
}

.progress-circle {
  position: absolute;
  top: 20px;
  left: 20px;
}

.pc-background {
  border-radius: 50%;
  width: 60px;
  height: 60px;
  background: #ddd;
  margin-bottom: 20px;
}

.pc-overlay {
  border-radius: 50%;
  width: 44px;
  height: 44px;
  font-size: 14px;
  background: #fff;
  position: absolute;
  margin-top: 8px;
  margin-left: 8px;
  display: flex;
  justify-content: center;
  align-items: center;
}

#new_game {
  border: 1px solid #ddd;
  box-shadow: 0px 0px 3px #1112;
  background: #fff;
  padding: 12px;
  cursor: pointer;
  transition: all 0.2s;
}

#new_game:hover {
  color: #208f8f;
  transform: scale(1.1);
}

#credits {
  font-size: 0.7rem;
  margin-top: 36px;
}

#credits div {
  margin: 0;
}

#credits a {
  color: darkcyan;
}

@media screen and (max-width: 1200px) {
  #app {
    width: 75vw;
  }
}

@media screen and (max-width: 768px) {
  #app {
    width: 90vw;
  }

  .word-display {
    width: 3.2vw;
  }
}
</style>
