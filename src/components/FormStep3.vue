<script setup lang="ts">
import type { Addon } from '@/types/Addon'
import { ref, type PropType, type Ref } from 'vue'

const props = defineProps({
  addons: {
    type: Array as PropType<Addon[]>,
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

const emit = defineEmits(['update:selected_addons'])

function toggleAddon(id: number) {
  props.selected_addons.includes(id) ? removeAddon(id) : addAddon(id)
}

function addAddon(id: number) {
  emit('update:selected_addons', props.selected_addons.concat([id]))
}

function removeAddon(id: number) {
  emit(
    'update:selected_addons',
    props.selected_addons.filter((addon_id) => addon_id != id),
  )
}
</script>

<template>
  <div class="step">
    <header>
      <h1>Pick add-ons</h1>
      <p>Add-ons help enhance your gaming experience.</p>
    </header>

    <div class="content">
      <div class="addons-container">
        <label v-for="(addon, index) in addons" :key="index" class="addon" :class="{ active: selected_addons.includes(index) }">
          <input type="checkbox" :checked="selected_addons.includes(index)" @click="toggleAddon(index)" />

          <div class="data">
            <div class="name">{{ addon.name }}</div>
            <p class="description">{{ addon.description }}</p>
          </div>

          <div class="price">+${{ addon.price[+bill_yearly] }}/{{ bill_yearly ? 'yr' : 'mo' }}</div>
        </label>
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

.addons-container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.addon {
  display: flex;
  align-items: center;
  gap: 2rem;
  flex-grow: 1;
  width: 100%;
  padding: 1rem 1.5rem;
  border: 1px solid hsl(229, 24%, 87%);
  border-radius: 0.5rem;
  color: #03295a;
}

.addon:hover,
.addon.active {
  border: 1px solid #03295a;
  cursor: pointer;
}

.addon .data {
  flex-grow: 1;
}

.addon .name {
  font-size: 1.125rem;
  font-weight: bold;
}

.addon p {
  margin-bottom: 0;
}

.addon input {
  min-width: 1.25rem;
  min-height: 1.25rem;
}

.price {
  color: hsl(243, 100%, 62%);
}

@media only screen and (max-width: 750px) {
  h1 {
    font-size: 1.5rem;
  }

  p {
    margin-bottom: 2rem;
  }

  .addon {
    gap: 1rem;
    padding: 1rem;
  }

  .addon .name {
    font-size: 0.875rem;
  }

  .addon p {
    font-size: 0.75rem;
  }

  .price {
    font-size: 0.75rem;
  }
}
</style>
