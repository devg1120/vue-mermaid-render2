<script setup lang="ts">
import { ref, onMounted, useTemplateRef } from "vue";

const props = defineProps([
  "tooltip",
  "name",
  "alignright",
  "radio_name",
  "radio_index",
  "radio_class",
]);

const emit = defineEmits([
  "toolbarItemClick",
  "toolbarItemToggle",
  "toolbarItemRadio",
]);
const item = useTemplateRef("item");

function item_mousedown(e) {
  emit("toolbarItemClick", props.name);
}
function item_mouseup(e) {}

function item_toggle_switch(e, state) {
  //emit('toolbarItemToggle', props.name, state, true)
  emit(
    "toolbarItemRadio",
    props.radio_name,
    props.radio_index,
    props.name,
    state,
  );
}

function radio_name() {
  return props.radio_name;
}
function radio_index() {
  return props.radio_index;
}
function reset() {
  const icons = item.value.querySelectorAll(".radioicon");
  icons.forEach((icon) => {
    icon.classList.remove("mouse-down");
  });
}

defineExpose({ radio_name, radio_index, reset });

onMounted(() => {
  const icons = item.value.querySelectorAll(".radioicon");
  icons.forEach((icon) => {
    icon.addEventListener("mouseover", function () {
      const targetIcon = this;
      const activeTooltip = targetIcon.parentElement.nextElementSibling;
      activeTooltip.classList.add("tip_active");
    });

    icon.addEventListener("mouseout", function () {
      const targetIcon = this;
      const activeTooltip = targetIcon.parentElement.nextElementSibling;
      activeTooltip.classList.remove("tip_active");
    });
    //icon.addEventListener("mousedown", item_mousedown) ;
    //icon.addEventListener("mouseup", item_mouseup);
    icon.addEventListener("mousedown", function (e) {
      const targetIcon = this;
      if (targetIcon.classList.contains("toggle")) {
        targetIcon.classList.toggle("mouse-down");
        if (targetIcon.classList.contains("mouse-down")) {
          item_toggle_switch(e, true);
        } else {
          item_toggle_switch(e, false);
        }
        return;
      }
      targetIcon.classList.add("mouse-down");
      item_mousedown(e);
    });
    icon.addEventListener("mouseup", function (e) {
      const targetIcon = this;
      if (targetIcon.classList.contains("toggle")) {
        return;
      }
      targetIcon.classList.remove("mouse-down");
      item_mouseup(e);
    });
  });
});
</script>

<template>
  <div
    class="item"
    ref="item"
    :class="{
      alignright: props.alignright,
      radio_start: props.radio_class == 'start',
      radio_mid: props.radio_class == 'mid',
      radio_end: props.radio_class == 'end',
    }"
  >
    <slot></slot>
    <div
      class="underline"
      :class="{
        radio_start: props.radio_class == 'start',
        radio_mid: props.radio_class == 'mid',
        radio_end: props.radio_class == 'end',
      }"
    ></div>
  </div>
  <div class="tooltip">
    <div class="tip_message">{{ props.tooltip }}</div>
  </div>
</template>

<style></style>

<style scoped>
.underline.radio_start {
  width: 31px;
  height: 4px;
  margin-top: -12px;
  margin-left: 2px;
  border-bottom: solid 3px lightgray;
  border-left: solid 3px lightgray;
}
.underline.radio_mid {
  width: 34px;
  height: 4px;
  margin-top: -12px;
  border-bottom: solid 3px lightgray;
}
.underline.radio_end {
  width: 33px;
  height: 4px;
  margin-top: -12px;
  margin-right: 2px;
  border-bottom: solid 3px lightgray;
  border-right: solid 3px lightgray;
}

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
