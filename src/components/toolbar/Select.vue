<script setup lang="ts">
import { ref } from "vue";
import VueSelect from "vue3-select-component";

const props = defineProps(["name", "options"]);

const emit = defineEmits(["toolbarItemSelect"]);

const selected = ref("");

function select(option) {
  console.log("*** selected:", option.value, selected.value);
  emit("toolbarItemSelect", props.name, selected.value);
}
</script>

<template>
  <div class="selectitem">
    <VueSelect
      v-model="selected"
      :options="props.options"
      placeholder="Select"
      @option-selected="select"
    >
      <template #option="{ option, index }">
        {{ option.label }} - {{ index }}
      </template>
    </VueSelect>
  </div>
</template>

<style scoped>
:deep(.control) {
  flex-wrap: nowrap !important;
}

:deep(.menu) {
  width: 160px !important;
}
</style>
