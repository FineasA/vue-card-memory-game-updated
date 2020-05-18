<template>
  <div>
    <b-card-group columns class="card-group">
      <b-card
        :class="{'disable-click' : disableClick}"
        :img-src="card.hidden ? cardBackURL : card.imgUrl"
        class="player-card"
        style="min-width: 16rem;"
        v-for="(card, index) in cards"
        :key="index"
        no-body
        @click="flipCard(index)"
      ></b-card>
    </b-card-group>
    <div class="score-display justify-content-center">
      <p class="score-text" style="margin-right: 5px">Number Correct: {{numberCorrect}}</p>
      <p class="score-text">Number Incorrect: {{numberIncorrect}}</p>
    </div>
    <b-alert
      style="text-align:center"
      show
      variant="warning"
    >Do not spam click! After selecting two photos, wait 2 seconds.</b-alert>
  </div>
</template>

<script>
export default {
  data() {
    return {
      disableClick: false,
      previous: null,
      mostRecent: null,
      numberCorrect: 0,
      numberIncorrect: 0,
      correctPairs: [],
      phaseOne: true,
      phaseTwo: false,
      isCorrect: false,
      cards: [],
      hidden: true,
      imgURLS: [
        {
          zion:
            "https://images.unsplash.com/photo-1473790034954-491b2ee68f30?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
        },
        {
          zion:
            "https://images.unsplash.com/photo-1473790123798-93586c8a5175?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjF9&auto=format&fit=crop&w=1950&q=80"
        },
        {
          zion:
            "https://images.unsplash.com/photo-1473789810014-375ed569d0ed?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=60"
        },
        {
          zion:
            "https://images.unsplash.com/photo-1474401915596-3c5adf84ef01?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=60"
        },
        {
          zion:
            "https://images.unsplash.com/photo-1477070730378-b48c1285afea?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60"
        },
        {
          zion:
            "https://images.unsplash.com/photo-1475351177616-1e5e440dccef?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60"
        }
      ],
      cardBackURL:
        "https://images.unsplash.com/photo-1574110537361-ff1948fc4a57?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
    };
  },
  methods: {
    createCards() {
      //create two identical card objects that are distinct and point to different references
      for (let i = 0; i < this.imgURLS.length; i++) {
        let cardOne = {
          hidden: this.hidden,
          cardId: i,
          imgUrl: this.imgURLS[i].zion
        };
        let cardTwo = {
          hidden: this.hidden,
          cardId: i,
          imgUrl: this.imgURLS[i].zion
        };
        this.cards.push(cardOne, cardTwo);
      }
      this.shuffleCards();
      console.log(this.cards);
    },
    shuffleCards() {
      for (let i = this.cards.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [this.cards[i], this.cards[j]] = [this.cards[j], this.cards[i]];
      }
    },
    flipCard(index) {
      //store the first click in a variable, then compare to next click
      if (this.phaseOne) {
        this.cards[index].hidden = false;
        this.previous = this.cards[index];
        console.log(this.previous);
        this.phaseOne = false;
        this.phaseTwo = true;
      } else if (this.phaseTwo) {
        this.cards[index].hidden = false;
        this.mostRecent = this.cards[index];
        console.log(
          "Previous: ",
          this.previous,
          "Most Recent: ",
          this.mostRecent
        );
        //begin testing to see if matched
        if (this.previous.cardId !== this.mostRecent.cardId) {
          console.log("incorrect");
          this.disableClick = true;
          this.numberIncorrect++;
          setTimeout(() => {
            console.log("cards should flip");
            this.$set(this.previous, "hidden", true);
            this.$set(this.mostRecent, "hidden", true);
            this.disableClick = false;
          }, 1500);
          if (this.phaseOne) {
            this.phaseOne = false;
            this.phaseTwo = true;
          } else if (this.phaseTwo) {
            this.phaseOne = true;
            this.phaseTwo = false;
          }
        } else if (this.previous.cardId === this.mostRecent.cardId) {
          console.log("correct");
          this.numberCorrect++;
          this.previous = {};
          this.mostRecent = {};
          this.phaseOne = true;
          this.phaseTwo = false;
        }
      }
    }
  },
  created() {
    console.log("created");
    this.createCards();
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Mukta&display=swap");

.player-card-hidden {
  visibility: hidden;
  border: 2px solid black;
}
.score-display {
  display: flex;
}
.score-text {
  font-family: "Mukta", sans-serif;
  font-size: 32px;
  color: black;
}

.disable-click {
  pointer-events: none;
}
</style>
