<script setup>
import "./style.css";

const { placeholder, value } = defineProps(["value", "placeholder"]);
const onChange = defineEmits(["update:modelValue"]);
</script>

<template>
  <input
    :value="value"
    type="text"
    id="itemInput"
    @input="(event) => onChange('update:modelValue', event.target.value)"
    :placeholder="placeholder"
  />
</template>

<style scoped></style>
