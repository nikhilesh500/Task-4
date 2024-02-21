<template>
  <div class="body">
    <section class="ConditionalComponent">
      <h1>Conditional Rendering</h1>
      <h2 v-show="ok">Welcome to the ICC one-day cricket league match</h2>
      
      <div class="one">
        <div class="toss" style="display: flex; gap: 10px;">
          <p>India have choosen Heads,</p>
          <button @click="value = !value">Toss</button>
          <p>{{ !value ? `It's Tails` : `It's Heads` }}</p>   
        </div>

        <div class="para" v-if="value">
          <h2>India Won the Toss</h2>
          <small>[ This is rendered as <u>value</u> is TRUE. ]</small>
        </div>
        <div class="para" v-else>
          <h2>India Lost the Toss</h2>
          <small>[ This is rendered as <u>value</u> is FALSE. ]</small>
        </div>
      </div>

      <div class="two">
        <button @click="type = 'ind'">India</button>
        <button @click="type = 'aus'">Australia</button>
        <button @click="type = ''">Re-match</button>
    
        <div class="para" v-if="type === 'ind'">
          <h2>India won the match</h2>
          <small>[ This is rendered as <u>type</u> is 'ind'. ]</small>
        </div>
        <div class="para" v-else-if="type === 'aus'">
          <h2>Australia won the match</h2>
          <small>[ This is rendered as <u>type</u> is 'aus'. ]</small>
        </div>
        <div class="para" v-else>
          <h2>Match not played yet.</h2>
          <small>[ This is rendered as <u>type</u> is neither 'ind' nor 'aus'. ]</small>
        </div>
      </div>
    </section>
    <section class="eventHandling">
      <h1>Event Handling</h1>
      <h2>Over-wise Scores</h2>
      <div>
        <button :disabled="isButtonDisabled" @click="generateRandomNumbers">Autoplay an over</button>
        <ul>
          <li v-for="(number, index) in randomNumbers" :key="index">
            {{ index + 1 }}'st Ball: {{ number }}
          </li>
        </ul>
      </div>
      <h2>Score: <span style="float: right;">{{ score }}/{{ wickets }}</span></h2>
      <h4 style="float: right;">Overs: {{ overs }} </h4>
    </section>
  </div>
</template>

<script setup>
import { ref,computed } from 'vue'

const value = ref('');
const type = ref('');
const ok = ref(true);
const overs = ref(0);
const score = ref(0);
const wickets = ref(0);
const randomNumbers = ref([]);
const generateRandomNumbers = () => {
  overs.value++;
  randomNumbers.value = Array.from({ length: 6 }, () => Math.floor(Math.random() * 8));
  randomNumbers.value.forEach((number, index) => {
    if (number === 7) {
      randomNumbers.value[index] = 'wicket';
      wickets.value++;
    }
    if (number !== 7) {
      score.value += number;
      console.log(score.value);
    }
  });
};

const isButtonDisabled = computed(() => {
  return wickets.value >= 10 || overs.value >= 10;
});
</script>

<style>
.body {
  display: flex;
  gap: 30px;
}
.toss{
  display: flex;
}
section h1 {
  text-align: center;
  font-weight: 500;
  color: #038bfa;
}
section {
  padding: 15px;
  backdrop-filter: blur(6px) saturate(59%);
  -webkit-backdrop-filter: blur(6px) saturate(59%);
  background-color: rgba(255, 255, 255, 0.45);
  border-radius: 12px;
  border: 1px solid rgba(209, 213, 219, 0.3);
}
button {
  padding: 5px 20px;
  border-radius: 10px;
  border: 1px solid rgb(0, 195, 255); 
  cursor: pointer;
  margin: auto 10px;
}
.one, .two {
  margin: 20px;
}
small {
  color: rgb(109, 109, 109);
}
</style>