<script setup lang="ts">
import type { PropType } from 'vue'

const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  phone: {
    type: String,
    required: true,
  },
  errors: {
    type: Array as PropType<string[]>,
    required: true,
  },
})

const emit = defineEmits(['update:name', 'update:email', 'update:phone'])
</script>

<template>
  <div class="step">
    <header>
      <h1>Personal info</h1>
      <p>Please provide your name, email address, and phone number.</p>
    </header>

    <div class="content">
      <label for="name">
        Name
        <div class="error" v-if="errors[0]">{{ errors[0] }}</div>
      </label>
      <input :class="{ wrong: errors[0] }" type="text" id="name" name="name" placeholder="e.g. Stephen King" :value="name" @keyup="emit('update:name', ($event.target as HTMLInputElement).value)" />

      <label for="email">
        Email Address
        <div class="error" v-if="errors[1]">{{ errors[1] }}</div>
      </label>
      <input :class="{ wrong: errors[1] }" type="email" id="email" name="email" placeholder="e.g. stephenking@lorem.com" :value="email" @keyup="emit('update:email', ($event.target as HTMLInputElement).value)" />

      <label for="phone-number">
        Phone Number
        <div class="error" v-if="errors[2]">{{ errors[2] }}</div>
      </label>
      <input :class="{ wrong: errors[2] }" type="tel" id="phone-number" name="phone-number" placeholder="e.g. +1 234 567 890" :value="phone" @keyup="emit('update:phone', ($event.target as HTMLInputElement).value)" />
    </div>
  </div>
</template>

<style scoped>
h1 {
  color: #03295a;
  font-size: 2rem;
  font-weight: bold;
}

p {
  margin-bottom: 3rem;
  color: hsl(231, 11%, 63%);
}

label {
  display: flex;
  justify-content: space-between;
  margin: 1.5rem 0 0.25rem 0;
  font-weight: 500;
  color: #03295a;
}

.error {
  margin-top: 0.1rem;
  font-size: 0.9rem;
  font-weight: bold;
  color: #d71e39;
}

input {
  width: 100%;
  padding: 0.5rem 1.125rem;
  border: 1px solid hsl(229, 24%, 87%);
  border-radius: 0.25rem;
  font-weight: 500;
  color: #03295a;
}

input:hover,
input:focus {
  border-color: #03295a;
  outline: none;
}

input.wrong {
  border-color: #d71e39;
}

@media only screen and (max-width: 750px) {
  h1 {
    font-size: 1.5rem;
  }

  p {
    margin-bottom: 2rem;
  }

  label {
    font-size: 0.9rem;
  }

  .error {
    font-size: 0.85rem;
  }
}
</style>
