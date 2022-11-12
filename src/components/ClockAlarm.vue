<template>
  <section class="parent">
    <section class="parent-1">
      <article id="article1">
        <div>
           <span id="break-label" class="bolder">
             Break Length
           </span>
           <div class="child-bolder">
            <span id="break-increment" class="clickable" @click="incrementBreaker(1,60, breaker)">⬆</span>
            <span id="break-length" class="dynamic">{{ breaker }}</span>
            <span id="break-decrement" class="clickable" @click="decrementBreaker(1,60, breaker)">⬇</span>
          </div>
        </div>
        <div>
          <span id="session-label" class="bolder">Set session</span>
          <div class="child-bolder">
            <span id="session-increment" class="clickable" @click="incrementSession(1,60, session)">⬆</span>
            <span id="session-length" class="dynamic">{{ session }}</span>
            <span id="session-decrement" class="clickable" @click="decrementSession(1,60, session)">⬇</span>
          </div>
        </div>
      </article>
      <article>
        <div class="session-container">
          <span id="timer-label"> Session </span>
          <div id="time-left">
            <span>{{ session }}:</span>
            <span><span v-if="minutes < 10">0</span>{{ minutes }}</span>
          </div>
        </div>
        <div class="last">
          <span id="start_stop" @click="[playAlarm = !playAlarm, starter()]"  class="clickable">🔝</span>
          <span id="reset" @click="resetAllTime" class="clickable">🔃</span>
        </div>
        <audio controls v-if="playAudio" :autoplay="playAudio" style="{ display: block; height: 30px; }" id="beep">
          <source src="http://upload.wikimedia.org/wikipedia/commons/8/86/20090724NIHWiki.ogg" type="audio/mpeg">
        </audio>
      </article>
    </section>
  </section>
</template>

<script setup lang="ts">
import { ref, watch} from "vue";

const breaker = ref<number>(5);
const session = ref<number>(25);

const minutes = ref<number>(0);
const playAudio = ref<boolean>(false);
const playAlarm = ref<boolean>(false);

const incrementSession = (min:number, max:number, currentTime:number): number => (currentTime > min && currentTime < max) ? session.value++ : 0;
const decrementSession = (min:number, max:number, currentTime:number): number => (currentTime > min && currentTime < max) ? session.value-- : 0;
const decrementBreaker = (min:number, max:number, currentTime:number): number => (currentTime > min && currentTime < max) ? breaker.value-- : 0;
const incrementBreaker = (min:number, max:number, currentTime:number): number => (currentTime > min && currentTime < max) ? breaker.value++ : 0;

watch(() => [minutes.value, session.value], () => {
  playAlarm.value = !minutes.value && !session.value;
});

const resetAllTime = (): void => {
  breaker.value = 5;
  session.value = 25;
}

const starter = (): void => {
  session.value = session.value - 1;
}
</script>

<style scoped>
.last {
  display: flex;
  margin-top: 10px;
  column-gap: 9px;
  margin-left:150px;
  max-width: 60px;
  justify-content: space-between;
}

audio {
  width: 170px;
  display: block;
  margin-left:80px;
  margin-top: 10px;
}

.dynamic {
  font-weight: bold;
  font-size: 20px;
}
.session-container {
  padding: 10px 0 10px 0;
  display: flex;
  flex-direction: column;
  margin-left: 70px;
  margin-top: 40px;
  row-gap: 4px;
}

#time-left{
  display: flex;
  justify-content: center;
  font-size: 28px;
  font-weight: bold;
}

#timer-label{
  font-weight: bold;
  color: black;
  font-size: 30px;
  display: flex;
  justify-content: center;
  text-align: center;
}

.session-container {
  border: 3px solid #0d0da9;
  border-radius: 7px;
  max-width: 200px;
}

.child-bolder {
  display: flex;
  column-gap: 25px;
  justify-content: center;
}

.bolder {
  font-weight: bold;
  color: #282424;
  font-size: 27px;
}

.parent {
  padding-top: 300px;
}

.parent-1 {
  display: flex;
  flex-direction: column;
  margin: auto;
  max-width: 300px;
}

#article1 {
  display: flex;
  justify-content: space-between;
}

.clickable {
  cursor: pointer;
  font-weight: bolder;
}
</style>