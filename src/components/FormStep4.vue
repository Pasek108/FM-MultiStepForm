<script setup lang="ts">
import type { Addon } from '@/types/Addon'
import type { Plan } from '@/types/Plan'
import { computed, type PropType } from 'vue'

const props = defineProps({
  plans: {
    type: Array as PropType<Plan[]>,
    required: true,
  },
  addons: {
    type: Array as PropType<Addon[]>,
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
  selected_addons: {
    type: Array as PropType<Number[]>,
    required: true,
  },
})

const emit = defineEmits(['update:selected_plan', 'update:bill_yearly', 'update:selected_addons', 'go_to_step_2'])

const total = computed(() => {
  let sum = props.plans[props.selected_plan].price[+props.bill_yearly]
  sum += props.selected_addons.reduce((val: number, id) => val + props.addons[id as number].price[+props.bill_yearly], 0)
  return sum
})
</script>

<template>
  <div class="step">
    <header>
      <h1>Finishing up</h1>
      <p>Double-check everything looks OK before confirming.</p>
    </header>

    <div class="content">
      <div class="summary">
        <header class="row">
          <div>
            <div class="name">{{ plans[+selected_plan].name }} ({{ bill_yearly ? 'Yearly' : 'Monthly' }})</div>
            <div class="change" @click="emit('go_to_step_2')">Change</div>
          </div>

          <div class="price">${{ plans[+selected_plan].price[+bill_yearly] }}/{{ bill_yearly ? 'yr' : 'mo' }}</div>
        </header>

        <hr />

        <div class="row" v-for="(addon_id, index) in selected_addons.sort()" :key="index">
          <p class="name">{{ addons[+addon_id].name }}</p>
          <div class="price">+${{ addons[+addon_id].price[+bill_yearly] }}/{{ bill_yearly ? 'yr' : 'mo' }}</div>
        </div>
      </div>

      <div class="total row">
        <span class="name">Total (per {{ bill_yearly ? 'year' : 'month' }}) </span>

        <div class="price">${{ total }}/{{ bill_yearly ? 'yr' : 'mo' }}</div>
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

.row {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
}

hr {
  border-color: #dddfe4;
}

.summary {
  display: flex;
  flex-direction: column;
  gap: 0.875rem;
  padding: 1.25rem 1.5rem;
  border-radius: 0.5rem;
  background-color: hsl(225, 100%, 98%);
}

.summary header {
  align-items: center;
}

.summary header .price,
.summary header .name {
  font-size: 1.125rem;
  font-weight: 600;
  color: #03295a;
}

.change {
  font-size: 1rem;
  font-weight: normal;
  text-decoration: underline;
  color: hsl(231, 11%, 63%);
}

.change:hover {
  color: hsl(243, 100%, 62%);
  cursor: pointer;
}

.summary .name {
  margin: 0;
  text-transform: capitalize;
}

.summary .price {
  font-weight: 500;
  color: #03295a;
}

.total {
  margin-top: 1rem;
  padding: 1.25rem 1.5rem;
}

.total .name {
  color: hsl(231, 11%, 63%);
}

.total .price {
  font-size: 1.25rem;
  font-weight: bold;
  color: hsl(243, 100%, 62%);
}

@media only screen and (max-width: 750px) {
  h1 {
    font-size: 1.5rem;
  }

  p {
    margin-bottom: 2rem;
  }

  .summary {
    padding: 1rem 1.25rem;
  }

  .summary header .price,
  .summary header .name {
    font-size: 1rem;
  }

  .change {
    font-size: 0.925rem;
  }

  .name,
  .price {
    font-size: 0.875rem;
  }

  .total .name,
  .total .price {
    font-size: 1rem;
  }
}
</style>
