<template>
    <div class="screen">
        <div class="screen__inner" :style="{
            width: `${
            ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) * Math.sqrt(cardsContext.length)}px`,
        }">
            <card-lipped v-for="(card, index) in cardsContext" :key="index" 
            :ref="`card-${index}`"
            :imgBackFaceUrl="`images/${card}.png`"
            :card="{index, value: card}"
            :cardsContext="cardsContext"
            @onFlip="checkRule($event)"/>
        </div>
        <!-- <h1>InteractScreen here....</h1> -->
        
    </div>
    
</template>

<script>
import Card from "./Card.vue"
export default{
    props: {
        cardsContext: {
            type: Array,
            default: function(){
                return [];
            },
        },
    },

    components: {
       CardLipped: Card
    },

    data() {
        return {
            rules: [],
        };
    },

    methods: {
        checkRule(card) {
           if(this.rules.length ===2) return false;

           this.rules.push(card);
        //    console.log("Rulse",this.rules);
           
           if(this.rules.length ===2 && this.rules[0].value == this.rules[1].value){
                console.log("right..")
                // add class 'disable' to compnent card
                this.$refs[`card-${this.rules[0].index}`][0].onEnableDisableMode();
                this.$refs[`card-${this.rules[1].index}`][0].onEnableDisableMode();
                // reset rules to []
                this.rules = [];

                const disableElements = document.querySelectorAll(".screen .card.disabled");
                // console.log(disableElements);

                if(disableElements && disableElements.length === this.cardsContext.length - 2){
                    setTimeout(() => {
                        this.$emit("onFinish");
                    }, 920);
                   
                }
           }else if(this.rules.length ===2 && this.rules[0].value !== this.rules[1].value){
                console.log("wrong..")
                console.log(this.$refs[`card-${this.rules[0].index}`][0])
                  setTimeout(() => {
                    this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
                    this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
                    this.rules = [];
                }, 800);
           }else return false
           
        },

        
    }
}
</script>

<style lang="css" scoped>
    .screen{
        width: 100%;
        height: 100vh;
        position: absolute;
        top:0;
        left: 0;
        z-index: 2;
        background-color: var(--dark);
        color: var(--light);
    }
    .screen__inner {
        width: 424px;
        display: flex;
        flex-wrap: wrap;
        margin: 2rem auto;
    }
</style>