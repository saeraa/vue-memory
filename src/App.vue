<template>

  <div class="cards-container">
    <div v-for="animal in shuffledAnimals" :key="animal">
      <Card :animal="animal" @pickedCard="pickCard"></Card>
    </div>

    <div v-if="!gameOn" class="start-div">
      <h1>Hi! Let's play some memory!</h1>
      <button @click="getNewCards">Start game</button>
    </div>
    <button v-else @click="reset">Reset game</button>
  </div>
</template>
 
<script>
import JSConfetti from 'js-confetti'
import Card from "./components/Card.vue"
export default {
  name: "App",
  components: { Card },
  methods: {
    reset() {
      this.shuffledAnimals.length = 0
      this.score = 0
      setTimeout(() => {

        this.getNewCards()
      }, 300)
    },
    confetti() {
      const jsConfetti = new JSConfetti()
      jsConfetti.addConfetti({ emojis: this.arrayOfAnimals })
    },
    getNewCards() {
      !this.gameOn ? this.gameOn = true : null
      this.shuffledAnimals = [...this.arrayOfAnimals].sort(() => Math.random() - 0.5)
    },
    pickCard(target) {
      this.picks.push(target)
      target.parentElement.classList.add("visible")
      if (this.picks.length == 2) { setTimeout(this.checkForMatch, 500) }
    },
    checkForMatch() {
      if (this.picks.every(card => card.nextSibling.textContent === this.picks[0].nextSibling.textContent)) {
        this.score++
        this.picks.forEach(card => {
          card.nextSibling.style.opacity = 0.5
          card.parentElement.classList.add("picked")
        })
      } else {
        this.picks.forEach(card => card.parentElement.classList.remove("visible"))
      }

      this.picks.length = 0

      if (this.score === this.shuffledAnimals.length / 2) this.confetti()
    }
  },
  data() {
    return {
      score: 0,
      picks: [],
      cards: [],
      gameOn: false,
      arrayOfAnimals: ["🐯", "🐯", "🐰", "🐰", "🦞", "🦞", "🐸", "🐸", "🦄", "🦄", "🐲", "🐲"],
      shuffledAnimals: []
    }
  }
}
</script>