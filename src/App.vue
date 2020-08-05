<template>
    <div id="app" class="game-wrapper">
        <div class="buttons-wrapper">
            <div class="button red" :class="{'active':buttonToAnimate==1}" @click="handleClick(1)"></div>
            <div
                class="button green"
                :class="{'active':buttonToAnimate==2}"
                @click="handleClick(2)"
            ></div>
            <div class="button blue" :class="{'active':buttonToAnimate==3}" @click="handleClick(3)"></div>
            <div
                class="button yellow"
                :class="{'active':buttonToAnimate==4}"
                @click="handleClick(4)"
            ></div>
            {{series}}
        </div>
        <div class="diffcult-button-wrapper">
            <input
                type="radio"
                name="difficultLevel"
                value="1500"
                @click="difficultLevelClickHandler(1500)"
            /> easy
            <input
                type="radio"
                name="difficultLevel"
                value="1000"
                @click="difficultLevelClickHandler(100)"
            /> normal
            <input
                type="radio"
                name="difficultLevel"
                value="450"
                @click="difficultLevelClickHandler(450)"
            /> hard
        </div>
        <button @click="startGame()">Start</button>
    </div>
</template>
<script>
/* eslint-disable */

export default {
    data() {
        return {
            series: new Array(50),
            buttonDisabled: false,
            currentRound: 0,
            buttonToAnimate: 0,
            difficult: 1,
            clickCounter: 0,
        };
    },

    methods: {
        startGame(difficultLevel = 1500) {
          console.log(difficultLevel)
            //this.seriesInit();
            this.series = [
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
                1,
            ];
            this.difficult = difficultLevel;
            this.currentRound = 7;
            this.clickCounter = 0;
            this.animateSeries(this.series.slice(0, this.currentRound));
        },

        seriesInit() {
            this.series = new Array(50)
                .fill("")
                .map((elem) => Math.ceil(Math.random() * 4));
        },

        lightUp(button) {
            this.buttonToAnimate = button;
        },

        difficultLevelClickHandler(level) {
            this.startGame(level)
        },

        handleClick(button) {
            if (!this.buttonDisabled) {
                if (button !== this.series[this.clickCounter]) {
                    alert("looser");
                }

                if (this.clickCounter + 1 === this.currentRound) {
                    alert("alert new round");
                    this.clickCounter = 0;
                    this.currentRound++;
                    this.animateSeries(this.series.slice(0, this.currentRound));
                } else {
                    this.clickCounter++;
                }
            }
        },

        animateSeries(seriesToAnimate) {
            this.buttonDisabled = true;
            let timer = 0;
            seriesToAnimate.forEach((elem, ind) => {
                setTimeout(() => {
                    this.lightUp(elem);

                    if (ind === seriesToAnimate.length - 1) {
                        this.buttonDisabled = false;
                        this.lightUp(0);
                    }
                }, timer);

                setTimeout(() => {
                    this.lightUp(0);
                }, timer + this.difficult / 2);

                timer += this.difficult;
            });
        },
    },
};
</script>
<style>
.game-wrapper {
    max-width: 100%;
    width: 900px;
    margin: 0 auto;
    background-color: darkslategray;
    padding: 50px;
    box-sizing: border-box;
    display: flex;
}

.button {
    max-width: 80px;
    height: 80px;
    width: 100%;
    opacity: 0.6;
    transition: 0.2s;
    border-radius: 4px;
}

.button:hover,
.active {
    opacity: 1;
    cursor: pointer;
}
.red {
    background-color: red;
}

.blue {
    background-color: blue;
}

.green {
    background-color: green;
}

.yellow {
    background-color: yellow;
}

.buttons-wrapper {
    max-width: 200px;
    widows: 100%;
    display: flex;
    flex-wrap: wrap;
}
</style>
