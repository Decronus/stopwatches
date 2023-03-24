<template>
    <div class="stopwatch-wrap">
        <div class="stopwatch-timer" :style="{ color: activeStopwatch, borderBottom: `1px solid ${activeStopwatch}` }">
            {{ formatTime }}
        </div>

        <div class="stopwatch-buttons">
            <div class="stopwatch-play-button" v-if="stopped" @click="start">
                <svg width="17" height="20" viewBox="0 0 17 20" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M0 20V0L17 10L0 20Z" fill="#9E9E9E" />
                </svg>
            </div>

            <div class="stopwatch-pause-button" v-else @click="pause">
                <svg width="10" height="20" viewBox="0 0 10 20" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M3 0H0V20H3V0Z" :fill="activeStopwatch" />
                    <path d="M10 0H7V20H10V0Z" :fill="activeStopwatch" />
                </svg>
            </div>

            <div class="stopwatch-stop-button" @click="stop">
                <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <rect width="20" height="20" :fill="activeStopwatch" />
                </svg>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "stopwatch-comp",

    props: {
        hoursProp: {
            type: Number,
            default: 0,
        },

        minutesProp: {
            type: Number,
            default: 0,
        },

        secondsProp: {
            type: Number,
            default: 0,
        },
    },

    data() {
        return {
            interval: null,
            hours: this.hoursProp < 0 ? 0 : this.hoursProp,
            minutes: this.minutesProp < 0 ? 0 : this.minutesProp,
            seconds: this.secondsProp < 0 ? 0 : this.secondsProp,
            stopped: true,
        };
    },

    methods: {
        start() {
            if (+this.seconds === 0 && +this.minutes === 0 && +this.hours === 0) {
                return;
            }

            this.interval = setInterval(() => {
                if (+this.seconds === 0 && +this.minutes === 0 && +this.hours === 0) {
                    this.stop();
                    return;
                }

                if (Number(this.seconds) === 0) {
                    this.seconds = 59;

                    if (Number(this.minutes) === 0) {
                        this.minutes = 59;

                        if (Number(this.hours) !== 0) {
                            --this.hours;
                        }
                    } else {
                        --this.minutes;
                    }
                } else {
                    --this.seconds;
                }
            }, 1000);

            this.stopped = false;
        },

        pause() {
            clearInterval(this.interval);
            this.stopped = true;
        },

        stop() {
            clearInterval(this.interval);

            this.hours = 0;
            this.minutes = 0;
            this.seconds = 0;

            this.stopped = true;
        },
    },

    computed: {
        formatTime() {
            if (this.hours <= 9) {
                this.hours = "0" + Number(this.hours);
            }

            if (this.minutes <= 9) {
                this.minutes = "0" + Number(this.minutes);
            }

            if (this.seconds <= 9) {
                this.seconds = "0" + Number(this.seconds);
            }

            if (this.hours !== "00" && this.minutes !== "00") {
                return `${this.hours}:${this.minutes}:${this.seconds}`;
            }

            if (this.hours === "00" && this.minutes !== "00") {
                return `${this.minutes}:${this.seconds}`;
            }

            if (this.hours !== "00" && this.minutes === "00") {
                return `${this.hours}:${this.minutes}:${this.seconds}`;
            }

            if (this.hours === "00" && this.minutes === "00") {
                return this.seconds === "00" ? "0" : this.seconds;
            }

            return "00:00:00";
        },

        activeStopwatch() {
            return this.stopped ? "#9e9e9e" : "#FFFFFF";
        },
    },
};
</script>

<style scoped>
.stopwatch-wrap {
    display: flex;
    flex-direction: column;
    background: #696969;
    width: 225px;
    height: 120px;
}

.stopwatch-timer {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 22px;
    color: #9e9e9e;
    height: 60px;
    border-bottom: 1px solid #9e9e9e;
}

.stopwatch-buttons {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 48px;
    height: 60px;
}

.stopwatch-buttons div {
    cursor: pointer;
}

.input {
    font-size: 22px;
    background: none;
    outline: none;
    border: none;
    color: #9e9e9e;
    width: 24px;
    padding-right: 5px;
}
</style>
