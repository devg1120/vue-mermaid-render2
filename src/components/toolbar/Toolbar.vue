<script setup lang="ts">
import { ref, onMounted } from "vue";

import ToolbarItem8 from "./ToolbarItem.vue";
import ToolbarItemPic8 from "./ToolbarItemPic.vue";
import ToolbarItemRadio8 from "./ToolbarItemRadio.vue";
import ToolbarItemSelect7 from "./ToolbarItemSelect.vue";

const props = defineProps(["toolbar_define", "icon_flat_mode"]);

const emit = defineEmits([
  "toolbarItemClick",
  "toolbarItemToggle",
  "toolbarItemSelect",
  "toolbarItemSelectColor",
  "toolbarItemRadio",
]);

function toggle_handler(name, state) {
  emit("toolbarItemToggle", name, state);
}

function radio_handler(radio_name, radio_index, name, state) {
  emit("toolbarItemRadio", radio_name, radio_index, name, state);

  console.log("radio switch", radio_name, radio_index, name, state);
  for (let i = 0; i < radioref.value.length; i++) {
    console.log(radioref.value[i].radio_name());
    if (radioref.value[i].radio_name() == radio_name) {
      if (radioref.value[i].radio_index() != radio_index) {
        radioref.value[i].reset();
      }
    }
    //console.log(radioref.value[i]);
  }
}
function click_handler(name) {
  emit("toolbarItemClick", name);
}

function select_handler(name, data) {
  emit("toolbarItemSelect", name, data);
}
function select_color_handler(name, data) {
  emit("toolbarItemSelectColor", name, data);
}

onMounted(() => {
  // emit = defineEmits(props.handler_define)
});

const radioref = ref(null);
</script>

<template>
  <div class="toolbar-base">
    <template v-for="item in props.toolbar_define" v-bind:key="item">
      <ToolbarItem8
        v-if="!item.select && !item.radio && !item.pic"
        :tooltip="item.tooltip"
        :name="item.name"
        :alignright="item.alignright"
        @toolbarItemClick="click_handler"
        @toolbarItemToggle="toggle_handler"
      >
        <component
          v-bind:is="item.icon"
          :class="{
            icon: !props.icon_flat_mode,
            icon_flat: props.icon_flat_mode,
            leftspace: item.leftspace,
            toggle: item.toggle,
          }"
        />
      </ToolbarItem8>
      <ToolbarItemPic8
        v-if="!item.select && !item.radio && item.pic"
        :tooltip="item.tooltip"
        :name="item.name"
        :alignright="item.alignright"
        @toolbarItemClick="click_handler"
        @toolbarItemToggle="toggle_handler"
      >
        <component v-bind:is="item.icon" />
      </ToolbarItemPic8>
      <ToolbarItemRadio8
        v-if="!item.select && item.radio"
        ref="radioref"
        :tooltip="item.tooltip"
        :name="item.name"
        :alignright="item.alignright"
        @toolbarItemClick="click_handler"
        @toolbarItemToggle="toggle_handler"
        @toolbarItemRadio="radio_handler"
        :radio_name="item.radio_name"
        :radio_index="item.radio_index"
        :radio_class="item.radio_class"
      >
        <component
          v-bind:is="item.icon"
          class="radioicon"
          :class="{
            leftspace: item.leftspace,
            toggle: item.toggle,
            radio: item.radio,
          }"
        />
      </ToolbarItemRadio8>
      <ToolbarItemSelect7
        v-if="item.select"
        :tooltip="item.tooltip"
        :name="item.name"
	:isClearable="item.isClearable"
	:placeholder="item.placeholder"
        :alignright="item.alignright"
      >
        <component
          v-bind:is="item.icon"
          :name="item.name"
          :options="item.options"
	  :isClearable="item.isClearable"
	  :placeholder="item.placeholder"
          @toolbarItemSelect="select_handler"
          class="select"
          :class="{ leftspace: item.leftspace, toggle: item.toggle }"
        />
      </ToolbarItemSelect7>
    </template>
  </div>
</template>

<style></style>

<style scoped>
.toolbar-base {
  display: flex;
  margin-left: 0px;
  margin-right: 0px;
  padding-left: 10px;
  padding-right: 10px;
}

.icon {
  width: 22px;
  height: 22px;
  cursor: pointer;
  margin: 3px 2px;
  padding: 3px 4px;
  box-shadow: 2px 2px 0px 0 lightgray;
}

.icon_flat {
  width: 22px;
  height: 22px;
  cursor: pointer;
  margin: 3px 2px;
  padding: 3px 4px;
}

.radioicon {
  width: 22px;
  height: 22px;
  cursor: pointer;
  margin: 3px 2px;
  padding: 3px 4px;
}

.select {
  margin: 0px 2px;
  --vs-border: 0;
  --vs-border-radius: 0px;
  --vs-padding: 0px 4px;
  --vs-min-height: 25px;
  --vs-font-family: inherit;
  box-shadow: 2px 2px 0px 0 lightgray;
}

.menu {
  font-family: monospace;
}

.menu-option {
  font-family: monospace;
}

.icon.mouse-down {
  box-shadow: -2px -2px 0px 0px lightgray;
}
.icon_flat.mouse-down {
  box-shadow: -2px -2px 0px 0px lightgray;
}

.radioicon.mouse-down {
  box-shadow: -2px -2px 0px 0px lightgray;
}

.leftspace {
  margin-left: 20px;
}
</style>
