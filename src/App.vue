<template>
    <div>
        <div id="app" class="game-wrapper" :class="{'blur': isLose}">
            <div class="buttons-wrapper">
                <div
                    class="button red"
                    :class="{'active':buttonToAnimate==1}"
                    @click="handleClick(1)"
                ></div>
                <div
                    class="button green"
                    :class="{'active':buttonToAnimate==2}"
                    @click="handleClick(2)"
                ></div>
                <div
                    class="button blue"
                    :class="{'active':buttonToAnimate==3}"
                    @click="handleClick(3)"
                ></div>
                <div
                    class="button yellow"
                    :class="{'active':buttonToAnimate==4}"
                    @click="handleClick(4)"
                ></div>
            </div>
            <div class="game-options">
                <div class="difficult-button-wrapper">
                    <input
                        type="radio"
                        name="difficultLevel"
                        value="1500"
                        @click="difficultLevelClickHandler(1500)"
                        checked
                    /> easy
                    <input
                        type="radio"
                        name="difficultLevel"
                        value="1000"
                        @click="difficultLevelClickHandler(1000)"
                    /> normal
                    <input
                        type="radio"
                        name="difficultLevel"
                        value="450"
                        @click="difficultLevelClickHandler(450)"
                    /> hard
                </div>
                <p>Раунд: {{series.length}}</p>
                <button @click.prevent="restart" class="start-game-btn">Start</button>
            </div>
        </div>
        <div class="lose-alert" v-if="this.isLose">
            <p class="lose-alert-text">You lose :(</p>
            <button class="lose-alert-btn" @click.prevent="restart">ok</button>
        </div>
    </div>
</template>
<script>
/* eslint-disable */

export default {
    data() {
        return {
            buttonDisabled: true,
            difficult: 1500,
            series: [],
            clickCounter: 0,
            buttonToAnimate: 0,
            isLose: false,

            sounds: {
                buttonSounds: [
                    "https://shorturl.at/bnvD8",
                    "https://shorturl.at/nFKVZ",
                    "https://shorturl.at/blGX6",
                    "https://shorturl.at/aswS9",
                ],
                error: "https://shorturl.at/fgU17",
            },
        };
    },

    methods: {
        restart() {
            this.isLose = false;
            this.series = [];
            this.newRound();
        },

        newRound() {
            setTimeout(() => {
                this.clickCounter = 0;
                this.series.push(Math.ceil(Math.random() * 4));
                this.animateSeries(this.series);
            }, 1000);
        },

        animateSeries(seriesToAnimate) {
            this.buttonDisabled = true;
            let timer = 0;

            seriesToAnimate.forEach((elem, ind) => {
                this.lightUpWithTimer(elem, timer, timer + this.difficult / 2);

                setTimeout(() => {
                    this.buttonDisabled = false;
                }, this.difficult * this.series.length - 100);

                timer += this.difficult;
            });
        },

        lightUpWithTimer(button, timeout, duration) {
            setTimeout(() => {
                this.buttonToAnimate = button;
                this.playSound(this.sounds.buttonSounds[button - 1]);
            }, timeout);

            setTimeout(() => {
                this.buttonToAnimate = 0;
            }, duration);
        },

        difficultLevelClickHandler(level) {
            this.difficult = level;
        },

        playSound(button) {
            let audio = new Audio(button);
            audio.play();
        },

        handleClick(button) {
            if (!this.buttonDisabled) {
                if (button !== this.series[this.clickCounter]) {
                    this.isLose = true;
                    return;
                }

                this.lightUpWithTimer(button, 0, 500);

                if (this.clickCounter + 1 === this.series.length) {
                    this.newRound();
                } else {
                    this.clickCounter++;
                }
            } else {
                this.playSound(this.sounds.error);
            }
        },
    },
};
</script>

<style src="./App.scss" lang="scss">
</style>
