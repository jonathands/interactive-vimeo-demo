<script setup lang="ts">
import { ref } from 'vue';

const emit = defineEmits<{
  (e: 'submit', rating: number): void;
}>();

const rating = ref(0);
const hoveredRating = ref(0);

const handleSubmit = () => {
  if (rating.value > 0) {
    emit('submit', rating.value);
  }
};
</script>

<template>
  <div class="rating-overlay">
    <h2>Rate Your Experience</h2>
    <div class="stars">
      <div
        v-for="i in 5"
        :key="i"
        class="star"
        :class="{
          filled: i <= rating || i <= hoveredRating,
        }"
        @click="rating = i"
        @mouseenter="hoveredRating = i"
        @mouseleave="hoveredRating = 0"
      >
        ★
      </div>
    </div>
    <button
      class="submit-button"
      @click="handleSubmit"
      :disabled="rating === 0"
    >
      Submit Rating
    </button>
  </div>
</template>

<style scoped>
.rating-overlay {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  text-align: center;
}

.stars {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
  margin: 1rem 0;
}

.star {
  font-size: 2rem;
  cursor: pointer;
  color: #ddd;
  transition: color 0.2s;
}

.star.filled {
  color: #ffd700;
}

.submit-button {
  padding: 1rem 2rem;
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