<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        :flippedCount="flippedCount"
        :cardsContext="cardsContext"
        @onFlip="checkRule($event)"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./Card.vue";

export default {
  props: {
    cardsContext: {
      type: Array,
      default: () => [],
    },
  },
  components: {
    CardFlip,
  },
  data() {
    return {
      rules: [],
      flippedCount: 0,
      matchedPairs: 0,
    };
  },
  methods: {
    checkRule(card) {
      if (this.flippedCount >= 2) return false;

      this.flippedCount++;
      this.rules.push(card);

      if (this.flippedCount === 2) {
        if (this.rules[0].value === this.rules[1].value) {
          console.log("right ...");
          setTimeout(() => {
            this.disableCards();
            this.resetFlippedCount();
            this.matchedPairs++;
            if (this.matchedPairs === this.cardsContext.length / 2) {
              this.$emit("onFinish");
            }
          }, 800);
        } else {
          console.log("wrong ...");
          setTimeout(() => {
            this.closeCards();
            this.resetFlippedCount();
          }, 800);
        }
      }
    },
    closeCards() {
      this.rules.forEach((rule) => {
        const cardRef = this.$refs[`card-${rule.index}`];
        if (cardRef && cardRef[0]) {
          cardRef[0].onFlipCloseCard();
        }
      });
      this.rules = [];
    },
    disableCards() {
      this.rules.forEach((rule) => {
        const cardRef = this.$refs[`card-${rule.index}`];
        if (cardRef && cardRef[0]) {
          cardRef[0].onEnableDisableMode();
        }
      });
      this.rules = [];
    },
    resetFlippedCount() {
      this.flippedCount = 0;
    },
  },
};
</script>

<style scoped lang="css">
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
