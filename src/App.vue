<template>
  <div id="app">
    <header>
      <h1>Tamagotchi</h1>
    </header>

    <section id="controls_newgame" v-if="dead===false">
      <button @click="startGame">New Game</button>
    </section>

    <section id="happiness" class="container">
      <h3>Happiness</h3>
      <div class="healthbar">
        <div class="healthbar__value" :style="happyBarStyles"></div>
      </div>
    </section>


    <section id="fullness" class="container">
      <h3>Fullness</h3>
      <div class="healthbar">
        <div class="healthbar__value" :style="fullBarStyles"></div>
      </div>
    </section>

    <section id="hygiene" class="container">
      <h3>Hygiene</h3>
      <div class="healthbar">
        <div class="healthbar__value" :style="hygieneBarStyles"></div>
      </div>
    </section>


    <section id="energy" class="container">
      <h3>Energy</h3>
      <div class="healthbar">
        <div class="healthbar__value" :style="energyBarStyles"></div>
      </div>
    </section>

    <section class="container" v-if="dead===true">
      <h2>Game Over!</h2>
      <h3>Your tamagotchi died because it was too sad.</h3>
      <button @click="startGame">Start Over</button>
    </section>

    <section id="controls" v-else-if="dead===false">
      <button @click="play">Play</button>
      <button @click="feed">Feed</button>
      <button @click="clean">Clean</button>
      <button @click="sleep">Sleep</button>
    </section>

    <section id="log" class="container">
    <div>
      <h2>Resum de la partida</h2>
      <ul>
      <li v-for="logMessage in logMessages" :key="logMessage">
      </li>
      </ul>
    </div>
    </section>

  </div>
</template>

<script>

export default {

  data() {
    return {
      happiness: 100,
      fullness: 100,
      hygiene: 100,
      energy: 100,
      dead:false,
      logMessages: []
    };
  },
  computed: {
    happyBarStyles() {
      if (this.happiness < 0) {
        return { width: '0%' };
      }
      return { width: this.happiness + '%' };
    },
    fullBarStyles() {
      if (this.fullness < 0) {
        return { width: '0%' };
      }
      return { width: this.fullness + '%' };
    },
    hygieneBarStyles() {
      if (this.hygiene < 0) {
        return { width: '0%' };
      }
      return { width: this.hygiene + '%' };
    },
    energyBarStyles() {
      if (this.energy < 0) {
        return { width: '0%' };
      }
      return { width: this.energy + '%' };
    },

  },
  watch: {
    happiness(value) {
      if (value <= 0) {
        this.dead = true;
      } else if (value >= 100) {
        this.happiness = 100;
      }
    },
    fullness(value) {
      if (value <= 0) {
        this.fullness = 0;
      } else if (value >= 100) {
        this.fullness = 100;
      }
      }
    },
    hygiene(value) {
      if (value <= 0) {
        this.hygiene = 0;
      } else if (value >= 100) {
        this.hygiene = 100;
      }
    },
    energy(value) {
      if (value <= 0) {
        this.energy = 0;
      } else if (value >= 100) {
        this.energy = 100;
      }
  },
  methods: {
    startGame() {
      this.happiness = 20;
      this.fullness = 50;
      this.hygiene = 15;
      this.energy = 25;
      this.dead = false;
    },
    play() {
      const attackValue = this.getRandomValue(5, 12);
      this.happiness += attackValue;
      this.hygiene -= attackValue;
      this.energy -= attackValue;
      this.addLogMessage('Happiness', 'went up by', attackValue);
      this.addLogMessage('Hygiene', 'went down by', attackValue);
    },
    feed() {
      const attackValue = this.getRandomValue(8, 15);
      this.fullness += attackValue;
      this.hygiene -= attackValue;
      //this.addLogMessage('Fullness', 'went up by', attackValue);
      //this.addLogMessage('Hygiene', 'went down by', attackValue);
      //this.feed();
    },
    clean() {
      const attackValue = this.getRandomValue(10, 25);
      this.happiness -= attackValue;
      this.hygiene += attackValue;
      //this.addLogMessage('Happiness', 'went down by', attackValue);
      //this.addLogMessage('Hygiene', 'is back to', 100);
      //this.clean();
    },
    sleep() {
      this.energy = 100;
      //this.addLogMessage('Energy', 'is back to', 100);
      //this.sleep();
    },
    addLogMessage(who, what, value) {
      this.logMessages.unshift({
        actionBy: who,
        actionType: what,
        actionValue: value
      });
    },
    getRandomValue(min, max) {
      return Math.floor(Math.random() * (max - min)) + min;
    }
  }

};

</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: 'Jost', sans-serif;
}

body {
  margin: 0;
}

header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 0.5rem;
  background-color: #880017;
  color: white;
  text-align: center;
  margin-bottom: 2rem;
}

section {
  width: 90%;
  max-width: 40rem;
  margin: auto;
}

.healthbar {
  width: 100%;
  height: 40px;
  border: 1px solid #575757;
  margin: 1rem 0;
  background: #fde5e5;
}

.healthbar__value {
  background-color: #00a876;
  width: 100%;
  height: 100%;
}

.container {
  text-align: center;
  padding: 0.5rem;
  margin: 1rem auto;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 12px;
}

#title {
  width:100%;
  height: 100px;
  vertical-align: center;
  text-align: center;
  color:white;
  background-color:#880017;
}
#profe h2,
#alumne h2 {
  margin: 0.25rem;
}

#titol h1{
  color:white;
}

#controls_newgame {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}

#controls {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}

button {
  font: inherit;
  border: 1px solid #88005b;
  background-color: #88005b;
  color: white;
  padding: 1rem 2rem;
  border-radius: 12px;
  margin: 1rem;
  width: 12rem;
  cursor: pointer;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}

button:focus {
  outline: none;
}

button:hover,
button:active {
  background-color: #af0a78;
  border-color: #af0a78;
  box-shadow: 1px 1px 8px rgba(0, 0, 0, 0.26);
}

button:disabled {
  background-color: #ccc;
  border-color: #ccc;
  box-shadow: none;
  color: #3f3f3f;
  cursor: not-allowed;
}

#log ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

#log li {
  margin: 0.5rem 0;
}

.log--damage {
  color: red;
}

.log--alumne {
  color: #7700ff;
}

.log--profe {
  color: #da8d00;
}
.log--heal {
  color: green;
}
</style>
