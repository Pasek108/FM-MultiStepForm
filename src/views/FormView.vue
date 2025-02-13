<script setup lang="ts">
import FormStep1 from '@/components/FormStep1.vue'
import FormStep2 from '@/components/FormStep2.vue'
import FormStep3 from '@/components/FormStep3.vue'
import FormStep4 from '@/components/FormStep4.vue'
import FormStep5 from '@/components/FormStep5.vue'
import Sidebar from '@/components/Sidebar.vue'
import type { Addon } from '@/types/Addon'
import type { Plan } from '@/types/Plan'
import { ref, type Ref } from 'vue'

// step 1
const name = ref('')
const email = ref('')
const phone = ref('')
const errors = ref(['', '', ''])

// step 2
const bill_yearly = ref(false)
const selected_plan = ref(0)
const plans: Plan[] = [
  { name: 'arcade', price: [9, 90] },
  { name: 'advanced', price: [12, 120] },
  { name: 'pro', price: [15, 150] },
]

// step 3
const selected_addons: Ref<number[], number[]> = ref([])
const addons: Addon[] = [
  { name: 'Online service', description: 'Access to multiplayer games', price: [1, 10] },
  { name: 'Larger storage', description: 'Extra 1TB of cloud save', price: [2, 20] },
  { name: 'Customizable Profile', description: 'Custom theme on your profile', price: [2, 20] },
]

// form
const step = ref(1)

function goNext() {
  if (validate() != 0) return
  step.value++
}

function goBack() {
  step.value--
}

function validate() {
  const fields: { name: string; element: Ref<string, string>; regex: RegExp }[] = [
    { name: 'Name', element: name, regex: /^[a-zA-Z ]+$/ },
    { name: 'Email', element: email, regex: /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|.(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/ },
    { name: 'Phone', element: phone, regex: /^(\+|00)[1-9][0-9 \-\(\)\.]{7,32}$/ },
  ]

  fields.forEach((field, index) => {
    if (field.element.value.trim().length == 0) errors.value[index] = 'This field is required'
    else if (!field.regex.test(field.element.value.trim())) errors.value[index] = field.name + ' is not correct'
    else errors.value[index] = ''
  })

  return errors.value.reduce((len, val) => (len += val.length), 0)
}
</script>

<template>
  <form onsubmit="return false;">
    <Sidebar :step_number="Math.min(step, 4)" />

    <div class="steps">
      <FormStep1 v-if="step == 1" v-model:name="name" v-model:email="email" v-model:phone="phone" :errors="errors" />

      <FormStep2 v-if="step == 2" :plans="plans" v-model:selected_plan="selected_plan" v-model:bill_yearly="bill_yearly" />

      <FormStep3 v-if="step == 3" :addons="addons" :bill_yearly="bill_yearly" v-model:selected_addons="selected_addons" />

      <FormStep4 v-if="step == 4" :plans="plans" :addons="addons" v-model:selected_plan="selected_plan" v-model:bill_yearly="bill_yearly" v-model:selected_addons="selected_addons" @go_to_step_2="step = 2" />

      <FormStep5 v-if="step == 5" />

      <div class="buttons" v-if="step < 5">
        <button class="next-button" @click="goNext" :class="{ confirm: step == 4 }">{{ step == 4 ? 'Confirm' : 'Next Step' }}</button>
        <button class="back-button" @click="goBack" v-if="step > 1">Go Back</button>
      </div>
    </div>
  </form>
</template>

<style scoped>
form {
  display: flex;
  width: 60rem;
  max-width: 100%;
  padding: 1rem;
  border-radius: 0.875rem;
  background-color: #ffffff;
  box-shadow: 0px 5px 50px -20px hsl(231, 11%, 63%);
}

.steps {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 2.5rem 6rem 1rem 6rem;
  flex-grow: 1;
}

.buttons {
  display: flex;
  flex-direction: row-reverse;
  justify-content: space-between;
}

.back-button {
  font-weight: 500;
  color: hsl(231, 11%, 63%);
}

.back-button:hover {
  color: #03295a;
  cursor: pointer;
}

.next-button {
  padding: 0.75rem 2rem;
  border-radius: 0.375rem;
  background-color: #03295a;
  color: hsl(231, 100%, 99%);
}

.next-button:hover {
  background-color: #174a8b;
  cursor: pointer;
}

.next-button.confirm {
  background-color: hsl(243, 100%, 62%);
}

.next-button.confirm:hover {
  background-color: #938cfe;
  cursor: pointer;
}

@media only screen and (max-width: 920px) {
  .steps {
    padding: 2rem 1.5rem 1rem 1.5rem;
  }
}

@media only screen and (max-width: 750px) {
  form {
    margin-top: 5rem;
    padding: 0;
  }

  .steps {
    padding: 2rem 1.5rem 2.5rem 1.5rem;
  }

  .buttons {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    padding: 1rem;
    background-color: #ffffff;
  }

  .next-button {
    padding: 0.675rem 1.125rem;
  }
}
</style>
