<script setup lang="ts">
import { reactive } from 'vue';
import { useVuelidate } from '@vuelidate/core';
import { required, email } from '@vuelidate/validators';

const emit = defineEmits<{
  (e: 'submit', formData: any): void;
}>();

const formData = reactive({
  name: '',
  email: '',
  phone: ''
});

const rules = {
  name: { required },
  email: { required, email },
  phone: { required }
};

const v$ = useVuelidate(rules, formData);

const handleSubmit = async () => {
  const isValid = await v$.value.$validate();
  if (isValid) {
    emit('submit', formData);
  }
};
</script>

<template>
  <div class="form-overlay">
    <h2>Please Complete the Form</h2>
    <form @submit.prevent="handleSubmit">
      <div class="form-group">
        <label for="name">Name:</label>
        <input
          id="name"
          v-model="formData.name"
          type="text"
          :class="{ error: v$.name.$error }"
        />
        <span class="error-message" v-if="v$.name.$error">
          Name is required
        </span>
      </div>

      <div class="form-group">
        <label for="email">Email:</label>
        <input
          id="email"
          v-model="formData.email"
          type="email"
          :class="{ error: v$.email.$error }"
        />
        <span class="error-message" v-if="v$.email.$error">
          Valid email is required
        </span>
      </div>

      <div class="form-group">
        <label for="phone">Phone:</label>
        <input
          id="phone"
          v-model="formData.phone"
          type="tel"
          :class="{ error: v$.phone.$error }"
        />
        <span class="error-message" v-if="v$.phone.$error">
          Phone is required
        </span>
      </div>

      <button type="submit" class="submit-button">Submit</button>
    </form>
  </div>
</template>

<style scoped>
.form-overlay {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  max-width: 400px;
}

.form-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
}

input {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
}

input.error {
  border-color: red;
}

.error-message {
  color: red;
  font-size: 0.8rem;
  margin-top: 0.25rem;
}

.submit-button {
  width: 100%;
  padding: 1rem;
  background: #42b883;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 1rem;
}
</style>