<script setup>
import {onMounted, ref} from "vue";

const type = ref('')
onMounted(async () => {
  const sentences = [
    'Full-Stack JavaScript Developer',
    'Block-Chain Enthusiast',
  ];
  const wait = async (mili_sec) => await new Promise(r => setTimeout(r, mili_sec))
  const writeSentence = async (sentence) => {
    for (let i = 0; i < sentence.length; i++) {
      type.value += sentence[i];
      await wait(Math.random() * 180 + 50)
    }
    await wait(1600);
  }
  const clearSentence = async () => {
    while (type.value.length > 0) {
      type.value = type.value.slice(0, -1);
      await wait(Math.random() * 100 + 50);
    }
  }

  await wait(1500);
  const write = async () => {
    for (let i = 0; i < sentences.length; i++) {
      await writeSentence(sentences[i])
      await clearSentence();
    }
    write();
  }
  write();
})
</script>

<template>
  <div class="HomePage">
    <div class="center">
      <div class="type">
        <div class=" stack" style="--stacks: 3;">
          <span v-for="i in 3" :key="i" :style="{'--index': i -1}">{{ type }}</span>
          <div class="cursor"/>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
body {
  --color: #5d5d5d;
  overflow: hidden;
}

.HomePage {
  display: flex;
  min-height: 100vh;
  justify-content: start;
  align-items: center;
  text-align: left;
  padding-left: 20vw;
}

.center {
  color: var(--color);
  font-size: 1.5rem;
  display: flex;
  justify-content: center;
  align-items: start;
}

.type {
  position: relative;
  display: inline-block;
  width: fit-content;
  min-height: 1.5rem;
  font-size: 4rem;
}

.type .cursor {
  position: absolute;
  right: -6px;
  width: 4px;
  height: 100%;
  min-height: 80px;
  background: #535353;
  top: 50%;
  transform: translateY(-50%);
  animation: blink 1000ms ease-in infinite;
}

@keyframes blink {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

.stack {
  display: grid;
  grid-template-columns: 1fr;
}

.stack span {
  font-weight: bold;
  grid-row-start: 1;
  grid-column-start: 1;
  font-size: 5rem;
  --stack-height: calc(100% / var(--stacks) - 1px);
  --inverse-index: calc(calc(var(--stacks) - 1) - var(--index));
  --clip-top: calc(var(--stack-height) * var(--index));
  --clip-bottom: calc(var(--stack-height) * var(--inverse-index));
  clip-path: inset(var(--clip-top) 0 var(--clip-bottom) 0);
  animation: stack 340ms cubic-bezier(.46, .29, 0, 1.24) 1 backwards calc(var(--index) * 120ms), glitch 2s ease infinite 2s alternate-reverse;
}

.stack span:nth-child(odd) {
  --glitch-translate: 8px;
}

.stack span:nth-child(even) {
  --glitch-translate: -8px;
}

@keyframes stack {
  0% {
    opacity: 0;
    transform: translateX(-50%);
    text-shadow: -2px 3px 0 red, 2px -3px 0 blue;
  }
  60% {
    opacity: 0.5;
    transform: translateX(50%);
  }
  80% {
    transform: none;
    opacity: 1;
    text-shadow: 2px -3px 0 red, -2px 3px 0 blue;
  }
  100% {
    text-shadow: none;
  }
}

@keyframes glitch {
  0% {
    text-shadow: -2px 3px 0 red, 2px -3px 0 blue;
    transform: translate(var(--glitch-translate));
  }
  2% {
    text-shadow: 2px -3px 0 red, -2px 3px 0 blue;
  }
  4%, 100% {
    text-shadow: none;
    transform: none;
  }
}
</style>
