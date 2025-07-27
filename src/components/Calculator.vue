<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'

const storageKey = 'prefs';

const input = ref('72\n48\n18');
const boardLength = ref(96);
const calcd = ref<CalculatedValue[]>([]);

interface CalculatedValue {
  key: string;
  value: string;
}

interface Prefs {
  input: string;
  boardLength: number;
}

watch([input, boardLength], () => {
  const prefs : Prefs = {
    input: input.value,
    boardLength: boardLength.value,
  }
  localStorage.setItem(storageKey, JSON.stringify(prefs));
});

onMounted(() => {
  try {
    const stored = localStorage.getItem(storageKey);
    if (stored) {
      const parsed = JSON.parse(stored) as Prefs;
      input.value = parsed.input;
      boardLength.value = parsed.boardLength;
    }
  } catch (e) {
    console.error(e);
  }
  calculate();
})

const calculate = () => {
  const cuts = input.value
    .split('\n')
    .map(v => v.trim())
    .filter(v => !!v)
    .map(v => parseFloat(v))
    .filter(v => !isNaN(v) && v < boardLength.value);
    
  cuts.sort()
  cuts.reverse();

  let currentLength = 0;
  let boards = 0;
  cuts.forEach(cut => {
    if (currentLength + cut >= boardLength.value) {
      currentLength = 0;
      boards++;
    }
    currentLength += cut;
  });

  input.value = cuts.join('\n');
  calcd.value = [
    {
      key: 'Boards Needed',
      value: `${boards + 1}`
    },
    {
      key: 'Remaining On Last Board',
      value: `${boardLength.value - currentLength}`
    },
  ];
}

</script>

<template>
  <div class="calculator">
    <div class="input">
      <div class="input-item">
        <label for="boardLength">Stock Board Length</label>
        <input 
          placeholder="Stock Board Length"
          id="boardLength"
          v-model="boardLength"
          type="number" />
      </div>
      <div class="input-item">
        <label for="input">Cut List (One Per Line)</label>
        <textarea id="input" v-model="input"></textarea>
      </div>
      <button @click="calculate">Calculate</button>
    </div>
    <div class="output">
      <div 
        v-for="keyval in calcd" 
        :key="keyval.key" 
        class="output-keyvalue">
        <div class="output-key">
          {{ keyval.key }}:
        </div>
        <div class="output-value">
          {{ keyval.value }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.calculator {
  display: flex;
  flex-direction: column;
}
button {
  background: var(--tufts-blue);
  border: none;
  outline: none;
  border-radius: 5px;
  width: 100%;
  font-size: 1.5em;
  padding: 0.5em 0;
  color: inherit;
  cursor: pointer;
}
.input-item {
  margin: 0 0 1em 0;
  width: 100%;
}
input,textarea {
  width: 100%;
  border: solid 1px var(--antiflash-white);
  outline: none;
  font-size: 1em;
  font-family: inherit;
  background: none;
  color:inherit;
  padding: 0.25em;
}
textarea {
  height: 50vh;
}
.output {
  padding: 1em 0;
}
.output-keyvalue {
  display: flex;
  flex-direction: row;
}
.output-key {
  padding-right: 0.25em;
}
</style>
