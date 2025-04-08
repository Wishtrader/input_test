<script setup>
import Input from './components/Input/Input.vue'
import UserIcon from '@/components/Input/UserIcon.vue'
import {ref} from 'vue'

const name1 = ref('')
const name2 = ref('')
const inputRef = ref(null)

const rules = [
  v => !!v || 'This field is required',
  v => v.length >= 3 || 'Minimum 3 letters'
]

</script>

<template>
  <h1>Test input 1</h1>
  <Input
      ref="inputRef"
      v-model="name1"
      label="Name"
      placeholder="Enter Your Name"
      :icon="UserIcon"
      :rules="rules"
      required
      :validate-on-input="true"
      :show-error-icon="true"
      :show-icon="false"
  />
  <h1>Test input 2</h1>
  <Input
      ref="inputRef"
      v-model="name2"
      label="Name"
      placeholder="Enter Your Name"
      :icon="UserIcon"
      :rules="rules"
      required
      :validate-on-input="true"
      :show-error-icon="true"
      :show-icon="true"
  />
</template>

<style scoped>

h1 {
  font-size: 32px;
  text-transform: uppercase;
  text-align: center;
}

</style>
