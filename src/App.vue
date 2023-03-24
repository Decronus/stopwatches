<template>
    <div class="stopwatches-container">
        <stopwatch-comp
            v-for="(el, index) in stopwatchesList"
            :key="index"
            :hoursProp="el.hours"
            :minutesProp="el.minutes"
            :secondsProp="el.seconds"
        />

        <div class="add-stopwatch-button" @click="addStopwatch">
            <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M11.5 0H8.5V8.5H0V11.5H8.5V20H11.5V11.5H20V8.5H11.5V0Z"
                    fill="#9E9E9E"
                />
            </svg>
        </div>
    </div>
</template>

<script>
import StopwatchComp from "./components/StopwatchComp.vue";

export default {
    name: "App",
    components: {
        StopwatchComp,
    },

    data() {
        return {
            stopwatchesList: [
                { minutes: 2, seconds: 40 },
                { minutes: 25, seconds: 32 },
                { hours: 1, minutes: 25, seconds: 32 },
                { hours: 1, minutes: 20, seconds: 33 },
            ],
        };
    },

    methods: {
        addStopwatch() {
            this.stopwatchesList.push({
                hours: this.getRandomInt(0, 99),
                minutes: this.getRandomInt(0, 59),
                seconds: this.getRandomInt(0, 59),
            });
        },

        getRandomInt(min, max) {
            min = Math.ceil(min);
            max = Math.floor(max);
            return Math.floor(Math.random() * (max - min + 1)) + min;
        },
    },
};
</script>

<style>
@font-face {
    font-family: "Gotham Pro";
    src: local("Gotham Pro Regular"), local("Gotham-Pro-Regular"), url("./fonts/GothamPro.woff2") format("woff2"),
        url("./fonts/GothamPro.woff") format("woff"), url("./fonts/GothamPro.ttf") format("truetype");
    font-weight: 400;
    font-style: normal;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

#app {
    font-family: "Gotham Pro";
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    margin: 0 auto;
    margin-top: 72px;
    width: 775px;
}

body {
    background: #353638;
    color: #ffffff;
}

input {
    font-family: "Gotham Pro";
}

.stopwatches-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    width: 100%;
    column-gap: 50px;
    row-gap: 45px;
}

.add-stopwatch-button {
    display: flex;
    justify-content: center;
    align-items: center;
    background: #696969;
    width: 225px;
    height: 120px;
    cursor: pointer;
}

@media (max-width: 819px) {
    #app {
        max-width: 500px;
    }

    .stopwatches-container {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 540px) {
    #app {
        padding-left: calc(50% - 112.5px);
        padding-right: calc(50% - 112.5px);
    }

    .stopwatches-container {
        grid-template-columns: 1fr;
    }
}
</style>
