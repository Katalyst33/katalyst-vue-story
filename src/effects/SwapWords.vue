<template>
  <div
    class="text-4xl font-semibold tracking-tight sm:text-6xl"
    id="word-container"
  >
    <span v-if="firstPart" :class="textColor" class="px-2">{{
      firstPart
    }}</span>
    <span v-if="secondPart" class="text-primary-500">{{ secondPart }}</span>
    <span class="cursor">|</span>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref, computed } from "vue";

const words = ["No Premine", "No ICOs", "No VC", "Fair Launch"];
const typedWord = ref("");
let currentWordIndex = 0;
let charIndex = 0;
let typingSpeed = 150; // milliseconds

defineProps({
  textColor: {
    type: String,
    default: "text-gray-200",
  },
});

const typeWord = () => {
  if (charIndex < words[currentWordIndex].length) {
    typedWord.value += words[currentWordIndex][charIndex];
    charIndex++;
    setTimeout(typeWord, typingSpeed);
  } else {
    // Wait for 2 seconds before starting to delete the word
    setTimeout(deleteWord, 2000);
  }
};

const deleteWord = () => {
  if (typedWord.value.length > 0) {
    typedWord.value = typedWord.value.substring(0, typedWord.value.length - 1);
    setTimeout(deleteWord, typingSpeed);
  } else {
    // Move to the next word
    currentWordIndex = (currentWordIndex + 1) % words.length;
    charIndex = 0;
    // Start typing the next word
    setTimeout(typeWord, typingSpeed);
  }
};

onMounted(() => {
  setTimeout(typeWord, typingSpeed);
});

const firstPart = computed(() => typedWord.value.split(" ")[0]);
const secondPart = computed(() => {
  const parts = typedWord.value.split(" ");
  return parts.length > 1 ? parts[1] : "";
});
</script>

<style scoped>
.cursor {
  display: inline-block;
  margin-left: 5px;
  animation: blink 1s step-end infinite;
}

@keyframes blink {
  from,
  to {
    color: transparent;
  }
  50% {
    color: black;
  }
}
</style>
