<template>
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
      width: `${
        (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4
      }px`,
      perspective: `${
        ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4) * 2
      }px`,
    }"
  >
    <div
      class="card__inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="onToggleFlipCard"
    >
      <div class="card__face card__face--front">
        <div
          class="card-content"
          :style="{
            backgroundSize: `${
              (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
              4 /
              3
            }px ${
              (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card-content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: [String, Number, Array, Object],
    },
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    flippedCount: {
      type: Number,
      default: 0,
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isFlipped: false,
      isDisabled: false,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisabled || (this.flippedCount >= 2 && !this.isFlipped))
        return false;
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },
    onFlipCloseCard() {
      this.isFlipped = false;
    },
    onEnableDisableMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style scoped lang="css">
.card {
  /* width: 90px;
  height: 120px; */
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 0.5s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
  background-color: var(--dark);
}

.card__face--front .card-content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  /* background-size: 40px 40px; */
  height: 100%;
  width: 100%;
}

.card__face--back {
  background: var(--light);
  transform: rotateY(-180deg);
}

.card__face--back .card-content {
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
}

.card.disabled .card__inner {
  cursor: default;
}
</style>
