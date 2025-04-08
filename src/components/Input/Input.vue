<template>
  <div class="container">
    <div class="input-wrapper">
      <label
          v-if="label"
          :class="['input-label', errorMessage && 'label-error']"
      >
        {{ label }}<span v-if="required">*</span>
      </label>

      <div class="input-container">

     <span v-if="showIcon && icon" class="input-icon">
      <component :is="icon" />
    </span>

        <input
            ref="inputRef"
            type="text"
            v-model="inputValue"
            :placeholder="placeholder"
            :disabled="disabled"
            class="[
              icon && 'has-icon',
              errorMessage && 'input-error',
              ]"
            @blur="validateInput"
            @input="onInput"
        />

        <span v-if="showErrorIcon && errorMessage" class="error-icon">
        ❗
      </span>
      </div>

      <p v-if="errorMessage" class="error-text">{{ errorMessage }}</p>
    </div>
  </div>
</template>

<script setup>
import {ref, watch, computed, defineExpose} from 'vue'

const props = defineProps({
  modelValue: String,
  placeholder: String,
  label: String,
  icon: [Object, Function],
  rules: {
    type: Array,
    default: () => []
  },
  required: Boolean,
  customErrorMessage: {
    type: String,
    default: 'The field can\'t be empty*'
  },
  validateOnInput: Boolean,
  showErrorIcon: Boolean,
  disabled: Boolean,
  showIcon: { type: Boolean, default: false }
})

const emit = defineEmits(['update:modelValue'])

const inputRef = ref(null)
const inputValue = ref(props.modelValue || '')
const errorMessage = ref('')


watch(() => props.modelValue, val => {
  inputValue.value = val
})

watch(() => inputValue.value, val => {
  emit('update:modelValue', val)
  if (props.validateOnInput) validateInput()
})


const getValidationRules = computed(() => {
  const requiredRule = props.required
      ? [(v) => !!v || props.customErrorMessage]
      : []
  return [...requiredRule, ...props.rules]
})


const validateInput = () => {
  for (const rule of getValidationRules.value) {
    const result = rule(inputValue.value)
    if (result !== true) {
      errorMessage.value = result
      return false
    }
  }
  errorMessage.value = ''
  return true
}

const onInput = () => {
  if (props.validateOnInput) validateInput()
}

defineExpose({
  validate: validateInput
})
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 100%;
  margin: 0 auto;
  padding: 0 40px;
  max-width: 760px;
}

.input-wrapper {
  display: flex;
  flex-direction: column;
  margin-bottom: 1rem;
  width: 100%;
}

.input-label {
  font-size: 18px;
  font-weight: 500;
  color: #8C52FF;
  margin-left: 20px;
  margin-bottom: 6px;
}

.input-container {
  display: flex;
  width: 100%;
  position: relative;
}

input {
  width: 100%;
  padding: 25px;
  font-size: 20px;
  font-weight: 500;
  border-radius: 20px;
  border: 1px solid #d3d3d3;
  background-color: white;
  color: #1B0A40;
  transition: all 0.2s ease-in-out;
  outline: none;
}

input:focus {
  border-color: #a46ae2;
  box-shadow: 0 0 0 2px rgba(164, 106, 226, 0.2);
}

input:disabled {
  background-color: #f0f0f0;
  color: #aaa;
  border-color: #ddd;
  cursor: not-allowed;
}

.input-icon {
  position: absolute;
  left: 2px;
  top: 50%;
  transform: translateY(-50%);
}

.error-icon {
  position: absolute;
  right: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: red;
  font-size: 18px;
}

.error-text {
  font-size: 18px;
  color: red;
  margin-top: 4px;
  margin-left: 20px;
}
</style>
