<script setup lang="ts">
import { ref } from "vue";
import VueSelect from "vue3-select-component";

import { StopIcon } from "./@heroicons/vue/16/solid";
import { PaperAirplaneIcon } from "./@heroicons/vue/24/outline";

const props = defineProps(["name"]);

const emit = defineEmits(["toolbarItemSelect"]);

const select_color = ref("white");

//{ label: StopIcon , value: 'silver' },
/*
https://www.rapidtables.com/web/color/RGB_Color.html
*/

const options = [
  { label: "😀", value: "😀" },
  { label: "🙂", value: "🙂" },
  { label: "😅", value: "😅" },
  { label: "😁", value: "😁" },
  { label: "😁", value: "😁" },

  { label: "👌", value: "👌" },
  { label: "✌", value: "✌" },
  { label: "👍", value: "👍" },
  { label: "👍", value: "👍" },
  { label: "🚫", value: "🚫" },

  { label: "👀", value: "👀" },
  { label: "🙆", value: "🙆" },
  { label: "🙅", value: "🙅" },
  { label: "🙇", value: "🙇" },
  { label: "🚫", value: "🚫" },

  { label: "🎉", value: "🎉" },
  { label: "🎊", value: "🎊" },
  { label: "🎁", value: "🎁" },
  { label: "🎯", value: "🎯" },
  { label: "🚫", value: "🚫" },

  { label: "⛔", value: "⛔" },
  { label: "🚫", value: "🚫" },
  { label: "🔰", value: "🔰" },
  { label: "㊗", value: "㊗" },
  { label: "🚫", value: "🚫" },

  { label: "❓", value: "❓" },
  { label: "❗", value: "❗" },
  { label: "⭕", value: "⭕" },
  { label: "❌", value: "❌" },
  { label: "🚫", value: "🚫" },

  { label: "✅", value: "✅" },
  { label: "✔", value: "✔" },
  { label: "☑", value: "☑" },
  { label: "❎", value: "❎" },
  { label: "🚫", value: "🚫" },
];

const options_ = [
  { label: "", value: "white" },
  { label: "", value: "silver" },
  { label: "", value: "gray" },
  { label: "", value: "black" },

  { label: "", value: "red" },
  { label: "", value: "maroon" },
  { label: "", value: "yellow" },
  { label: "", value: "olive" },

  { label: "", value: "lime" },
  { label: "", value: "green" },
  { label: "", value: "aqua" },
  { label: "", value: "teal" },

  { label: "", value: "blue" },
  { label: "", value: "navy" },
  { label: "", value: "fuchsia" },
  { label: "", value: "purple" },
];
const selected = ref("");

function select(option) {
  console.log("*** selected:", option.value, selected.value);
  select_color.value = option.value;
  emit("toolbarItemSelect", props.name, selected.value);
}

function set_style(color) {
  const style = "background-color:" + color;
  return style;
}
</script>

<!--
https://vue3-select-component.vercel.app/slots.html
-->

<template>
  <div class="selectitem">
    <VueSelect
      v-model="selected"
      :options="options"
      placeholder=""
      @option-selected="select"
      :isClearable="false"
      :isSearchable="false"
    >
      <template #option="{ option, index }">
        <!--
	    <div :style="styles[index]" class="item" >&ensp;&ensp;&ensp;</div>
	    -->
        <!--
	    <div :style="set_style(option.value)" class="item" >&ensp;&ensp;&ensp;</div>
	    -->
        <div class="emoji">{{ option.label }}</div>
      </template>
    </VueSelect>
  </div>
</template>

<style scoped>
:deep(.control) {
  flex-wrap: nowrap !important;
}

:deep(.emoji) {
}
/*
:deep(.item) {
   width:160px !important;
}
*/
:deep(.menu) {
  display: grid;
  /*
  width: 150px !important;
  grid-template-columns: repeat(auto-fill, minmax(30px, 1fr));

  width: 170px !important;
  grid-template-columns: repeat(auto-fill, minmax(35px, 1fr));
*/
  /*
  width: 180px !important;
  grid-template-columns: repeat(auto-fill, minmax(37px, 1fr));
*/
  width: 180px !important;
  grid-template-columns: repeat(auto-fill, minmax(32px, 1fr));
}
:deep(.menu-option) {
  /*--vs-option-padding: 8px 12px; */
  --vs-option-padding: 4px 6px;
}

:deep(.single-value) {
  padding: 2px 0px 3px 0px;
  margin: 0px;
  height: 22px;
}

/*
:deep(.value-container) {
  margin-top:4px;
    margin-left:6px;
}

:deep(.input-container) {
  background-color: v-bind(select_color);

  width:16px;
  height:16px;

}
*/

:deep(.value-container) {
  margin-top: 5px;
}

:deep(.input-container) {
  background-color: v-bind(select_color);
  /* color: v-bind(select_color); */
  margin-top: 5px;
  margin-left: 2px;
  width: 16px;
  height: 16px;
}
</style>
