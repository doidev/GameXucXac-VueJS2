<template>
  <div id="app">
    <div class="wrapper clearfix">
      <Players
        v-bind:isWinner="isWinner"
        v-bind:scoresPlayer="scoresPlayer"
        v-bind:activePlayer="activePlayer"
        v-bind:currentScore="currentScore"
      />
      <Controls
        v-bind:isPlaying="isPlaying"
        v-bind:finalScore="finalScore"
        v-on:handleChangeFinalScore="handleChangeFinalScore"
        v-on:handleNewGame="handleNewGame"
        v-on:handleRollDice="handleRollDice"
        v-on:handleHoldDiceHoldScore="handleHoldDiceHoldScore"
      />
      <Dices v-bind:dices="dices" />
      <PopupRule
        v-bind:isOpenPopup="isOpenPopup"
        v-on:handleClosePopup="handleClosePopup"
      />
    </div>
  </div>
</template>

<script>
import Players from "./components/Players.vue";
import Controls from "./components/Controls.vue";
import Dices from "./components/Dices.vue";
import PopupRule from "./components/PopupRule.vue";

export default {
  name: "app",
  data() {
    return {
      isOpenPopup: false,
      isPlaying: false,
      scoresPlayer: [0, 0],
      currentScore: 0,
      activePlayer: 0,
      dices: [2, 6],
      finalScore: 10,
    };
  },
  components: {
    Players,
    Controls,
    Dices,
    PopupRule,
  },
  computed: {
    isWinner() {
      let {scoresPlayer, finalScore} = this;
      if (scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore){
        //
        this.isPlaying = false
        return true;
      }
      return false
    }
  }
  ,
  methods: {
    nextPlayer() {
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      this.currentScore = 0;
    },
    handleNewGame() {
      this.isOpenPopup = true;
    },
    handleClosePopup() {
      this.isPlaying = true;
      this.activePlayer = 0;
      this.scoresPlayer = [0, 0];
      this.currentScore = 0;
      this.isOpenPopup = false;
      this.dices = [1, 1];
    },
    handleRollDice() {
      if (this.isPlaying) {
        const dice1 = Math.floor(Math.random() * 6) + 1;
        const dice2 = Math.floor(Math.random() * 6) + 1;
        this.dices = [dice1, dice2];
        if ((dice1 === 1) | (dice2 === 1)) {
          setTimeout(() => {
            alert(
              `Ngoi choi Player ${this.activePlayer +
                1} da xoay trung so 1. Rat tiec`
            );
          }, 10);
          this.nextPlayer();
        } else {
          this.currentScore = this.currentScore + dice1 + dice2;
        }
      } else {
        alert("Please start New Game");
      }
    },
    handleHoldDiceHoldScore() {
      if (this.isPlaying) {
        let { scoresPlayer, activePlayer, currentScore } = this;
        let oldScore = scoresPlayer[activePlayer];
        this.$set(this.scoresPlayer, activePlayer, oldScore + currentScore);
        // let cloneScorePlayer = [...scoresPlayer];
        // cloneScorePlayer[activePlayer] = oldScore + currentScore;
        // this.scoresPlayer = cloneScorePlayer;
        if(!this.isWinner){
        this.nextPlayer();
        }
      } else {
        alert("Please start New Game");
      }
    },
    handleChangeFinalScore(e) {
      let number = parseInt(e.target.value);
      if (isNaN(number)) {
        this.finalScore = "";
      } else {
        this.finalScore = number;
      }
    },
  },
};
</script>

<style>
/**********************************************
*** GENERAL
**********************************************/

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.clearfix::after {
  content: "";
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(
      rgba(62, 20, 20, 0.4),
      rgba(62, 20, 20, 0.4)
    ),
    url("/public/assets/back.jpg");
  background-size: cover;
  background-position: center;
  font-family: Lato;
  font-weight: 300;
  position: relative;
  height: 100vh;
  color: #555;
}

.wrapper {
  width: 1000px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}
</style>
