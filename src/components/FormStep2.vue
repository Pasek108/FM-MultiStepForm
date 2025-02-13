<script setup lang="ts">
import type { Plan } from '@/types/Plan'
import type { PropType } from 'vue'

const props = defineProps({
  plans: {
    type: Array as PropType<Plan[]>,
    required: true,
  },
  selected_plan: {
    type: Number,
    required: true,
  },
  bill_yearly: {
    type: Boolean,
    required: true,
  },
})

const emit = defineEmits(['update:selected_plan', 'update:bill_yearly'])
</script>

<template>
  <div class="step">
    <header>
      <h1>Select your plan</h1>
      <p>You have the option of monthly or yearly billing.</p>
    </header>

    <div class="content">
      <div class="cards-container">
        <div v-for="(plan, index) in plans" :key="index" class="card" :class="{ active: selected_plan == index }" @click="emit('update:selected_plan', index)">
          <img :src="'images/icon-' + plan.name + '.svg'" alt="{{ plan.name }} icon" />

          <div class="card-data">
            <div class="name">{{ plan.name }}</div>
            <div class="price">${{ plan.price[+bill_yearly] }}/{{ bill_yearly ? 'yr' : 'mo' }}</div>
            <div class="free" :style="{ display: bill_yearly ? '' : 'none' }">2 months free</div>
          </div>
        </div>
      </div>

      <div class="period">
        <span :class="{ active: !bill_yearly }">Monthly</span>

        <label for="yearly" class="period-toggle" :class="{ active: bill_yearly }">
          <input type="checkbox" id="yearly" name="yearly" :value="props.bill_yearly" @input="emit('update:bill_yearly', ($event.target as HTMLInputElement).checked)" />
        </label>

        <span :class="{ active: bill_yearly }">Yearly</span>
      </div>
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

.cards-container {
  display: flex;
  gap: 1rem;
}

.card {
  flex-grow: 1;
  width: 25%;
  padding: 1rem;
  border: 1px solid hsl(229, 24%, 87%);
  border-radius: 0.5rem;
}

.card:hover,
.card.active {
  border: 1px solid #03295a;
  cursor: pointer;
}

.name {
  display: block;
  margin-top: 2.5rem;
  font-size: 1rem;
  font-weight: bold;
  color: #03295a;
  text-transform: capitalize;
}

.price {
  font-size: 0.875rem;
  color: hsl(231, 11%, 63%);
}

.free {
  font-size: 0.875rem;
  color: #03295a;
}

.period {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1.375rem;
  margin-top: 2rem;
  padding: 0.5rem;
  border-radius: 0.5rem;
  background-color: hsl(225, 100%, 98%);
}

.period span {
  font-weight: 500;
  color: hsl(231, 11%, 63%);
  transition: color 0.25s;
}

.period span.active {
  color: #03295a;
}

.period-toggle {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 2.5rem;
  height: 1.25rem;
  padding: 0.25rem;
  border-radius: 99in;
  background-color: #03295a;
  cursor: pointer;
}

.period-toggle:after {
  content: '';
  position: absolute;
  left: 0.25rem;
  width: 0.75rem;
  height: 0.75rem;
  border-radius: 99in;
  background-color: #fcffff;
  transition: translate 0.5s;
}

.period-toggle.active:after {
  translate: 1.25rem;
}

.period-toggle input {
  display: none;
}

@media only screen and (max-width: 750px) {
  h1 {
    font-size: 1.5rem;
  }

  p {
    margin-bottom: 2rem;
  }

  .cards-container {
    flex-direction: column;
  }

  .card {
    display: flex;
    align-items: start;
    gap: 1rem;
    width: 100%;
  }

  .name {
    margin-top: 0;
  }
}
</style>
