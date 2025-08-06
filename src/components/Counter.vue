<template>
    <div id="countdown-container" v-if="loaded">

        <section id="header">
            <h4 v-if="!expired">New Year Countdown</h4>
            <h4 v-else>Timer is Done</h4>
        </section>

        <section id="countdown">
            <div>{{ displayDays }}
                <div class="label">days</div>
            </div>
            <span class="leading-snug">:</span>
            <div>{{ displayHours }}
                <div class="label">hours</div>
            </div>
            <span class="leading-snug">:</span>
            <div>{{ displayMinutes }}
                <div class="label">minutes</div>
            </div>
            <span class="leading-snug">:</span>
            <div>{{ displaySeconds }}
                <div class="label">seconds</div>
            </div>

        </section>

    </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';

const displayDays = ref(0);
const displayHours = ref(0);
const displayMinutes = ref(0);
const displaySeconds = ref(0);
const loaded = ref(false);
const expired = ref(false);
const props = defineProps(['year', 'month', 'date', 'hour', 'minute', 'second', 'millisecond'])


const timeValues = computed(() => {
    const seconds = 1000;
    const minutes = seconds * 60;
    const hours = minutes * 60;
    const days = hours * 24;

    return { seconds, minutes, hours, days };
});
const end = () => {
    return new Date(props.year, props.month - 1, props.date, props.hour, props.minute, props.second, props.millisecond);
};


const showRemaining = () => {
    const timer = setInterval(() => {
        const now = new Date();
        const endDateTime = end();
        const distance = endDateTime.getTime() - now.getTime();


        if (distance < 0) {
            clearInterval(timer);
            expired.value = true;
            console.log('Timer Expired');
            return;
        }

        displayDays.value = Math.floor(distance / timeValues.value.days);
        displayHours.value = Math.floor((distance % timeValues.value.days) / (timeValues.value.hours));
        displayMinutes.value = Math.floor((distance % timeValues.value.hours) / (timeValues.value.minutes));
        displaySeconds.value = Math.floor((distance % (timeValues.value.minutes)) / timeValues.value.seconds);
        loaded.value = true;
    }, 1000);
};

onMounted(() => {
    showRemaining();
});
</script>




<style lang="scss" scoped>
#countdown-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-family: 'Acorn Regular';
    gap:2vw;


    #header >* {
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 10vw;
        margin: 0 !important;
    }

    #countdown {
        display: flex;
        flex-direction: row;
        font-size: 9vw;
        font-family: 'Acorn Regular';
        position: relative;

        div {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: end;
        }

        div+span {
            margin: 0 5px 0 5px;
        }

        .label {
            font-size: 1.5vw;
            position: absolute;
        }

        .leading-snug {
            text-align: center;
        }
    }
}
</style>