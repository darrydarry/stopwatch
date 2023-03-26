<script setup>
import { computed, onDeactivated, ref } from 'vue';

const timeInterval = ref()
const isTimeRunning = ref(false)
const seconds = ref(0)
const minutes = ref(0)
const hours = ref(0)
const displayTimeOnPause = ref(0)
const emit = defineEmits(["removeStopwatch"])

const displayTime = computed(() => {
    let stringTime = ''

    if (hours.value) {
        stringTime += hours.value + ":"
    }

    if (minutes.value) {
        stringTime += (minutes.value > 9 ? minutes.value : "0" + minutes.value) + ":"
    }

    if (seconds.value) {
        stringTime += seconds.value > 9 ? seconds.value : "0" + seconds.value
    }

    return stringTime
})

onDeactivated(() => {
    if (timeInterval.value) {
        clearInterval(timeInterval.value)
    }
})


const startTime = () => {
    if (displayTimeOnPause.value) {
        const numbers = displayTimeOnPause.value.split(":")

        if (numbers.length === 3) {
            const [hour, minute, second] = numbers
            hours.value = hour
            minutes.value = minute
            seconds.value = second
        }

        if (numbers.length === 2) {
            const [minute, second] = numbers
            minutes.value = minute
            seconds.value = second
        }

        if (numbers.length === 1) {
            const [second] = numbers
            seconds.value = second
        }

    }
    isTimeRunning.value = true;
    timeInterval.value = setInterval(() => {
        if (isTimeRunning.value) {
            seconds.value++;
            if (seconds.value === 60) {
                seconds.value = 0;
                minutes.value++;
            }
            if (minutes.value === 60) {
                minutes.value = 0;
                hours.value++;
            }
        }
    }, 1000);
}

const pauseTime = () => {
    isTimeRunning.value = false
    displayTimeOnPause.value = displayTime.value

    clearInterval(timeInterval.value)
    timeInterval.value = null

    seconds.value = 0
    minutes.value = 0
    hours.value = 0

}

const removeStopwatch = () => {
    emit('removeStopwatch')
}

</script>

<template>
    <div class="stop-watch">
        <div class="display-time">
            <div v-if="displayTime">{{ displayTime }} </div>
            <div v-else>{{ displayTimeOnPause }}</div>
        </div>
        <div class="buttons">
            <button class="button start" v-if="!isTimeRunning" @click="startTime" :disabled="isTimeRunning" />
            <button class="button pause" v-else @click="pauseTime" :disabled="!isTimeRunning" />

            <button class="button stop" @click="removeStopwatch" />
        </div>

    </div>
</template>

<style scoped>
.stop-watch {
    background-color: #696969;
    width: 225px;
    height: 120px;
    min-width: 225px;
}

.display-time {
    display: flex;
    justify-content: center;
    color: #9E9E9E;
    font-size: 22px;
    line-height: 21px;
    padding: 20px 0;
    border-bottom: 1px solid #9E9E9E;
}

.buttons {
    display: flex;
    justify-content: center;
    padding: 20px 0;
}

.button {
    cursor: pointer;
}

.button.start {
    width: 17px;
    height: 20px;
    border-style: solid;
    border-width: 37px;
    box-sizing: border-box;
    border-width: 9px 0px 9px 17px;
    background: #696969;
    border-color: transparent transparent transparent #9E9E9E;
}

.button.pause {
    width: 17px;
    height: 20px;
    border-color: #9E9E9E;
    background: #696969;
    border-style: double;
    border-width: 0px 0px 0px 17px;
}

.button.stop {
    width: 20px;
    height: 20px;
    background-color: #9E9E9E;
    border: none;
}
</style>
