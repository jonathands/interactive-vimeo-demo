<script setup lang="ts">
import { ref } from 'vue';

interface Question {
  text: string;
  options: string[];
  correctAnswer: number;
}

const props = defineProps<{
  question: Question;
}>();

const emit = defineEmits<{
  (e: 'submit', isCorrect: boolean): void;
}>();

const selectedOption = ref<number | null>(null);

const handleSubmit = () => {
  if (selectedOption.value !== null) {
    emit('submit', selectedOption.value === props.question.correctAnswer);
  }
};
</script>

<template>
  <div class="question-overlay">
    <h2>{{ question.text }}</h2>
    <div class="options">
      <div
        v-for="(option, index) in question.options"
        :key="index"
        class="option"
        :class="{ selected: selectedOption === index }"
        @click="selectedOption = index"
      >
        {{ option }}
      </div>
    </div>
    <button
      class="submit-button"
      @click="handleSubmit"
      :disabled="selectedOption === null"
    >
      Submit Answer
    </button>
  </div>
</template>

<style scoped>
.question-overlay {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  max-width: 500px;
}

.options {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin: 1rem 0;
}

.option {
  padding: 1rem;
  border: 2px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s;
}

.option:hover {
  background: #f5f5f5;
}

.option.selected {
  border-color: #42b883;
  background: #e8f5e9;
}

.submit-button {
  width: 100%;
  padding: 1rem;
  background: #42b883;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.submit-button:disabled {
  background: #ccc;
  cursor: not-allowed;
}
</style>