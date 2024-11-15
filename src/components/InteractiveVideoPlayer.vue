<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import Player from '@vimeo/player';
import QuestionOverlay from './overlays/QuestionOverlay.vue';
import FormOverlay from './overlays/FormOverlay.vue';
import RatingOverlay from './overlays/RatingOverlay.vue';

const player = ref<Player | null>(null);
const currentTime = ref(0);
const activeOverlay = ref<string | null>(null);

const overlayTriggers = {
  question1: { time: 9, type: 'question1' },
  question2: { time: 20, type: 'question2' },
  form: { time: 30, type: 'form' },
  rating: { time: 40, type: 'rating' },
};

const questions = {
  question1: {
    text: 'What is the main topic discussed in the video so far?',
    options: ['Option 1', 'Option 2', 'Option 3', 'Option 4', 'Option 5'],
    correctAnswer: 2
  },
  question2: {
    text: 'Which of the following best describes the concept presented?',
    options: ['Option 1', 'Option 2', 'Option 3', 'Option 4', 'Option 5', 'Option 6'],
    correctAnswer: 3
  }
};

onMounted(() => {
  player.value = new Player('vimeo-player', {
    id: 226053498,
    width: 800
  });

  player.value.on('timeupdate', ({ seconds }: { seconds: number }) => {
    currentTime.value = seconds;
    checkOverlayTriggers(seconds);
  });
});

onUnmounted(() => {
  player.value?.destroy();
});

const checkOverlayTriggers = (time: number) => {
  for (const [key, trigger] of Object.entries(overlayTriggers)) {
    if (Math.floor(time) === trigger.time && !activeOverlay.value) {
      player.value?.pause();
      activeOverlay.value = trigger.type;
      break;
    }
  }
};

const handleQuestionSubmit = (isCorrect: boolean) => {
  if (isCorrect) {
    activeOverlay.value = null;
    player.value?.play();
  } else {
    player.value?.setCurrentTime(currentTime.value - 5);
  }
};

const handleFormSubmit = (formData: any) => {
  console.log('Form submitted:', formData);
  activeOverlay.value = null;
  player.value?.play();
};

const handleRatingSubmit = (rating: number) => {
  console.log('Rating submitted:', rating);
  activeOverlay.value = null;
  player.value?.play();
};
</script>

<template>
  <div class="video-container">
    <div id="vimeo-player"></div>
    
    <div v-if="activeOverlay" class="overlay-container">
      <QuestionOverlay
        v-if="activeOverlay === 'question1'"
        :question="questions.question1"
        @submit="handleQuestionSubmit"
      />
      
      <QuestionOverlay
        v-if="activeOverlay === 'question2'"
        :question="questions.question2"
        @submit="handleQuestionSubmit"
      />
      
      <FormOverlay
        v-if="activeOverlay === 'form'"
        @submit="handleFormSubmit"
      />
      
      <RatingOverlay
        v-if="activeOverlay === 'rating'"
        @submit="handleRatingSubmit"
      />
    </div>
  </div>
</template>

<style scoped>
.video-container {
  position: relative;
  width: 800px;
  margin: 0 auto;
}

.overlay-container {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>