<template>
  <div id="app">
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
              :class="{'btn-game__btn-active':isActiveRed}"
            ></button>
          </li>
          <li>
            <button
              type="button"
              class="btn-game__btn btn-game__blue"
              @click="pressButton('blue')"
              :class="{'btn-game__btn-active':isActiveBlue}"
            ></button>
          </li>
          <li>
            <button
              type="button"
              class="btn-game__btn btn-game__yellow"
              @click="pressButton('yellow')"
              :class="{'btn-game__btn-active':isActiveYellow}"
            ></button>
          </li>
          <li>
            <button
              type="button"
              class="btn-game__btn btn-game__green"
              @click="pressButton('green')"
              :class="{'btn-game__btn-active':isActiveGreen}"
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
          <button type="button" class="option__btn" @click="startGame" :disabled="disabled">Start</button>
          <p class="opion__title-list-option">Game options:</p>
          <ul class="option__list">
            <li class="opton__item">
              <input type="radio" name="level" id="easy" checked />
              <label for="easy">Easy</label>
            </li>
            <li class="opton__item">
              <input type="radio" name="level" id="middle" />
              <label for="middle">Middle</label>
            </li>
            <li class="opton__item">
              <input type="radio" name="level" id="hard" />
              <label for="hard">Hard</label>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      disabled: false,
      isActiveRed: false,
      isActiveBlue: false,
      isActiveYellow: false,
      isActiveGreen: false,
      counter: 0,
      lengthOrder: 15,
      highlighItem: "",
      allQueue: [],
      easyMode: 1500,
      middleMode: 1000,
      hardMode: 400,
    };
  },

  methods: {
    pressButton(userColor) {
      if (this.allQueue.length > 0 && !this.disabled) {
        let color = this.allQueue.shift();
        console.log(color);
        if (color !== userColor) {
          this.gameOver();
        } else if (this.allQueue.length === 0) {
          this.levelComplete();
        }
      } else {
        this.levelComplete();
      }
    },

    // this.allQueue.filter((el) => el != userColor);
    // if (this.allQueue === 0) {
    //   this.levelComplete();
    // }
    // }

    // else {
    //   this.levelComplete();
    // }

    gameOver() {
      this.counter = 0;
      this.allQueue = [];
      console.log("Игра окончена");
    },

    levelComplete() {
      this.counter++;
      this.allQueue = [];
      this.randomColor();
      this.playQueue([...this.allQueue]);
    },

    randomColor() {
      let nextLevel = this.counter;
      for (let i = 0; i < nextLevel; i++) {
        this.createQueue();
      }
    },

    startGame() {
      this.disabled = true;
      this.counter = 1;
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
        setTimeout(this.playQueue, this.easyMode, array);
      }
    },

    flickerButton(el) {
      this.highlightButton(el);
      setTimeout(this.colorReturn, 300, el);
    },

    highlightButton(el) {
      switch (el) {
        case "red":
          this.isActiveRed = true;
          break;
        case "blue":
          this.isActiveBlue = true;
          break;
        case "yellow":
          this.isActiveYellow = true;
          break;
        case "green":
          this.isActiveGreen = true;
          break;
      }
    },

    colorReturn(el) {
      switch (el) {
        case "red":
          this.isActiveRed = false;
          break;
        case "blue":
          this.isActiveBlue = false;
          break;
        case "yellow":
          this.isActiveYellow = false;
          break;
        case "green":
          this.isActiveGreen = false;
          break;
      }
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

.btn-game__btn:hover
  border: 5px solid #000000

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
  margin-bottom: 20px
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
