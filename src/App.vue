<template>
  <div id="app">
    <div class="wrapper">
      <div class="app__container-title">
        <p class="app__title">Simon says</p>
      </div>
      <div class="container">
        <div class="container__btn">
          <ul class="btn-game">
            <li>
              <button
                type="button"
                class="btn-game__btn btn-game__red"
                @click="pressButton('red')"
                :class="{'btn-game__btn-active':isActiveColor === 'red'}"
                :disabled="disabledButton"
              ></button>
            </li>
            <li>
              <button
                type="button"
                class="btn-game__btn btn-game__blue"
                @click="pressButton('blue')"
                :class="{'btn-game__btn-active':isActiveColor === 'blue'}"
                :disabled="disabledButton"
              ></button>
            </li>
            <li>
              <button
                type="button"
                class="btn-game__btn btn-game__yellow"
                @click="pressButton('yellow')"
                :class="{'btn-game__btn-active':isActiveColor === 'yellow'}"
                :disabled="disabledButton"
              ></button>
            </li>
            <li>
              <button
                type="button"
                class="btn-game__btn btn-game__green"
                @click="pressButton('green')"
                :class="{'btn-game__btn-active':isActiveColor === 'green'}"
                :disabled="disabledButton"
              ></button>
            </li>
          </ul>
        </div>
        <div class="container__option">
          <div class="option">
            <p class="option__round">
              Round:
              <span class="option__round-conter">{{ counter }}</span>
            </p>
            <button
              type="button"
              class="option__btn"
              @click="startGame"
              :disabled="disabledStart"
            >Start</button>
            <p v-if="alertGameOver" class="option__game-over">Game over</p>
            <p v-if="alertCongratulation" class="option__game-complete">You Win!</p>
            <p class="opion__title-list-option">Game options:</p>
            <ul class="option__list">
              <li class="opton__item">
                <input type="radio" name="level" id="easy" value="1500" v-model="mode" />
                <label for="easy">Easy</label>
              </li>
              <li class="opton__item">
                <input type="radio" name="level" id="middle" value="1000" v-model="mode" />
                <label for="middle">Middle</label>
              </li>
              <li class="opton__item">
                <input type="radio" name="level" id="hard" value="400" v-model="mode" />
                <label for="hard">Hard</label>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},

  data() {
    return {
      disabledButton: true,
      disabledStart: false,
      isActiveColor: "",
      alertGameOver: false,
      alertCongratulation: false,
      counter: 0,
      lengthQueue: 20,
      allQueue: [],
      mode: 1500,
    };
  },

  methods: {
    pressButton(userColor) {
      if (this.disabledButton) {
        return;
      } else if (this.allQueue.length > 0) {
        let color = this.allQueue.shift();
        this.flickerButton(userColor);
        if (color !== userColor) {
          this.gameOver();
        } else if (this.allQueue.length === 0) {
          this.disabledButton = true;
          this.levelComplete();
        }
      } else {
        this.levelComplete();
      }
    },

    gameOver() {
      this.counter = 0;
      this.allQueue = [];
      this.disabledStart = false;
      this.disabledButton = true;
      this.alertGameOver = true;
    },

    levelComplete() {
      if (this.counter !== this.lengthQueue) {
        setTimeout(this.nextLevel, 1500);
      } else {
        this.counter = 0;
        this.allQueue = [];
        this.disabledStart = false;
        this.alertCongratulation = true;
      }
    },

    nextLevel() {
      this.counter++;
      this.allQueue = [];
      this.randomColor();
      this.playQueue([...this.allQueue]);
    },

    randomColor() {
      for (let i = 0; i < this.counter; i++) {
        this.createQueue();
      }
    },

    startGame() {
      this.alertCongratulation = false;
      this.alertGameOver = false;
      this.disabledStart = true;
      this.counter++;
      this.createQueue();
      this.playQueue([...this.allQueue]);
    },

    createQueue() {
      let colorNumber = Math.floor(Math.random() * 4 + 1);
      const colors = {
        1: "red",
        2: "blue",
        3: "yellow",
        4: "green",
      };

      this.allQueue.push(colors[colorNumber]);
    },

    playQueue(array) {
      let arrayElement = array.shift();
      this.flickerButton(arrayElement);
      if (array.length > 0) {
        setTimeout(this.playQueue, this.mode, array);
      } else {
        this.disabledButton = false;
      }
    },

    flickerButton(el) {
      this.highlightButton(el);
      setTimeout(this.colorReturn, 300, el);
    },

    highlightButton(el) {
      this.isActiveColor = el;
      let audio = new Audio(require(`./assets/sound/${el}.mp3`));
      audio.play();
    },

    colorReturn(el) {
      this.isActiveColor = "";
    },
  },
};
</script>

<style lang="sass">
@import "./style.sass"

.app__container-title
  display: flex
  justify-content: center
  margin-top: 20px
  margin-bottom: 220px

.app__title
  font-size: 45px
  font-weight: 700

.container
  display: flex
  justify-content: space-between
  width: 550px
  margin: 0 auto

.container__btn
  position: relative
  width: 300px
  height: 295px

.btn-game__btn
  position: absolute
  width: 295px
  height: 290px
  border-radius: 150px 150px 150px 150px
  cursor: pointer
  opacity: 0.5
  outline: none

.btn-game__btn:active
  opacity: 1

.btn-game__btn-active
  opacity: 1

.btn-game__red
  background: #ff0000
  clip: rect(0px, 300px, 150px, 150px)

.btn-game__blue
  background: #1e90ff
  clip: rect(0px, 150px, 150px, 0px)

.btn-game__yellow
  background: #ffff00
  clip: rect(150px, 150px, 300px, 0px)

.btn-game__green
  background: #00ff00
  clip: rect(150px,300px, 300px, 150px)

.option__game-over
  font-size: 20px
  font-weight: 700
  color: #ff0000
  margin-bottom: 5px

.option__game-complete
  font-size: 20px
  font-weight: 700
  color: #00ff7f
  margin-bottom: 5px

.option__round
  font-size: 27px
  font-weight: 700
  margin-top: 30px
  margin-bottom: 20px

.option__btn
  font-size: 25px
  font-weight: 700
  width: 120px
  height: 35px
  margin-bottom: 5px
  border-radius: 10px
  background-color: #00ffff

.option__btn:hover
  background-color: #1e90ff

.opion__title-list-option
  font-size: 25px
  font-weight: 700
  margin-bottom: 5px

.opton__item
  font-size: 18px
  font-weight: 500

.opton__item input, label
  margin-right: 15px
  cursor: pointer
</style>
