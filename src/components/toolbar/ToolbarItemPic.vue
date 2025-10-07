<script setup lang="ts">
import { ref, onMounted, useTemplateRef } from "vue";
</script>

<template>
  <div class="item" ref="item">
    <slot></slot>
  </div>
</template>

<style></style>

<style scoped>
.tooltip {
  position: relative;
  top: 40px;
  left: -20px;
  display: none;
}

.tooltip .tip_message {
  position: absolute;
  margin-top: 4px;
  border-radius: 8px;
  background-color: #3399ff;
  color: #ffffff;
  font-size: 14px;
  line-height: 1.2;
  padding: 8px 14px;
}

.tip_active {
  display: block;
}

.alignright {
  margin-left: auto;
}
</style>
