<template>
    <div class="card" :class="{disabled:isDisable}"
    :style="{
        height: `${(720 - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
        width: `${
        (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4}px`,
        perspective: `${
        ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4) * 2
      }px`,
    }">
        <div class="card__inner" :class="{'is-flipped': isFlipped}" @click="onToggleFilpCard">
            <div class="card__face card__face--front">
                <div class="card__content" :style="{
            'background-size': `${
              (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
              4 /
              3
            }px ${
              (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
              4 /
              3
            }px`,
          }"></div>
            </div>

            <div class="card__face card__face--black">
                <div class="card__content" :style="{backgroundImage:`url(${require('@/assets/' + imgBackFaceUrl)})`}"></div>
            </div>
        </div>
    </div>
</template>
<script>
export default{
    props: {
        card: {
            type: [Array, String, Number, Object],
        },
        imgBackFaceUrl:{
            type: String,
            required: true,
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
            isDisable: false,
            isFlipped: false,
        }
    },
    methods: {
        onToggleFilpCard(){
            if(this.isDisable) return false;
            this.isFlipped = !this.isFlipped;
            if(this.isFlipped) this.$emit("onFlip", this.card);
        },

        onFlipBackCard() {
            this.isFlipped = false;
        },

        onEnableDisableMode() {
            this.isDisable = true;
        }
    },
};
</script>
<style lang="css" scoped>
.card {
    display: inline-block;
    margin-right: 1rem;
    margin-bottom: 1rem;
    width: 90px;
    height: 120px;
}
.card__inner{
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
}

.card.disabled .card__inner{
    cursor: default;
}
.card__inner.is-flipped {
    transform: rotateY(-180deg);
}
.card__face{
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    padding: 1rem;
    box-shadow: 0 3px 10px 3px rgba(0, 0, 0, .2);
}
.card__face--front .card__content {
    background: url("../assets/images/icon_back.png") no-repeat center center;
    /* background-size: 40px 40px; */
    height: 100%;
    width: 100%;
}
.card__face--black{
    background-color: var(--light);
    transform: rotateY(-180deg);
}
.card__face--black .card__content{
    background-size: contain;
    background-position: center;
    background-repeat: no-repeat;
    height: 100%;
    width: 100%;
}
</style>