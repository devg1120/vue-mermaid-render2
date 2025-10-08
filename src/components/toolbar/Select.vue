<script setup lang="ts">
import { ref } from "vue";
import VueSelect from "vue3-select-component";

const props = defineProps(["name", "options", "placeholder", "isClearable"]);

const emit = defineEmits(["toolbarItemSelect"]);

const selected = ref("");

function select(option) {
  console.log("*** selected:", option.value, selected.value);
  emit("toolbarItemSelect", props.name, selected.value);
}

const isClearable = false;

</script>
<!--

      :placeholder="props.placeholder"
      :isClearable="props.isClearable"
      placeholder="light"
      :isClearable="false"

-->
<template>
  <div class="selectitem">
    <VueSelect
      v-model="selected"
      :options="props.options"
      :placeholder="props.placeholder"
      :isClearable="false"
      @option-selected="select"
    >
      <template #option="{ option, index }">
        {{ option.label }} 
      </template>
    </VueSelect>
  </div>
</template>

<style scoped>
.selectitem {
}

:deep(.vue-select) {
}
:deep(.control) {
  flex-wrap: nowrap !important;
}

:deep(.menu) {
  width: 160px !important;
}
</style>
