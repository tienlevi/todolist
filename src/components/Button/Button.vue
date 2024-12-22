<script setup>
import "./style.css";

const { children, onClick } = defineProps(["children", "onClick"]);
</script>

<template>
  <button type="button" class="btn" id="clear-list" @click="onClick">
    {{ children }}
  </button>
</template>

<style scoped></style>
