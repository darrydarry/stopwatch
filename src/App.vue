<script setup>
import { ref } from 'vue'
import StopwatchComponent from './components/StopwatchComponent.vue'
const stopWatchers = ref([])
const uniqId = ref(0)
const removeStopwatch = (id) => {
  const newstopWatchers = stopWatchers.value.filter((el) => el.id !== id)
  console.log(newstopWatchers)
  stopWatchers.value = newstopWatchers
}
const addStopwatch = () => {
  stopWatchers.value.push({ id: uniqId.value })
  uniqId.value++
}
</script>

<template>
  <main class="container">
    <div class="watches-wrapp">
      <div class="">
        <div class="wrapp">
          <StopwatchComponent
            v-for="stopWatcher in stopWatchers"
            :key="stopWatcher.id"
            class="stop-watch"
            @removeStopwatch="() => removeStopwatch(stopWatcher.id)"
          />
          <div class="square plus-wrap" @click="() => addStopwatch()">
            <div class="plus" />
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.watches-wrapp {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.wrapp {
  display: flex;
  flex-wrap: wrap;
  width: 825px;
}

.stop-watch {
  margin-bottom: 45px;
  margin-right: 50px;
}

@media screen and (min-width: 1024px) {
  .stop-watch:nth-child(3n + 3) {
    margin-right: 0;
  }
}

@media screen and (min-width: 768px) and (max-width: 1023px) {
  .stop-watch:nth-child(2n + 2) {
    margin-right: 0;
  }

  .wrapp {
    width: 500px;
  }
}

@media screen and (max-width: 767px) {
  .stop-watch {
    margin-right: 0;
  }

  .wrapp {
    width: 100%;
    display: block;
  }
}

.plus-wrap {
  position: relative;
  background-color: #696969;
  width: 225px;
  height: 120px;
  min-width: 225px;
}

.plus {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;

  width: 20px;
  height: 20px;
  background: linear-gradient(to bottom, transparent 35%, #9e9e9e 35%, #9e9e9e 65%, transparent 65%),
    linear-gradient(to right, transparent 35%, #9e9e9e 35%, #9e9e9e 65%, transparent 65%);
}
</style>
