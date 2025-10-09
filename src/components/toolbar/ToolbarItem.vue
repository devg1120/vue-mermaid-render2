<script setup lang="ts">
import { ref, onMounted, useTemplateRef } from "vue";

const props = defineProps(["tooltip", "name", "alignright"]);

const emit = defineEmits(["toolbarItemClick", "toolbarItemToggle"]);
const item = useTemplateRef("item");

defineExpose({ name, isToggle , toggle_off, class_remove});


function item_mousedown(e) {
  emit("toolbarItemClick", props.name);
}
function item_mouseup(e) {}

function item_toggle_switch(e, state) {
  emit("toolbarItemToggle", props.name, state);
}

function name() {
  return props.name;
}
function isToggle() {
  let icons = item.value.querySelectorAll(".icon");
  return icons[0].classList.contains("toggle");
}
function toggle_off() {
  let icons = item.value.querySelectorAll(".icon");
  icons[0].classList.remove("mouse-down");
}
function class_remove(classname) {
  let icons = item.value.querySelectorAll(".icon");
  icons[0].classList.remove(classname);
}

onMounted(() => {
  let icons = item.value.querySelectorAll(".icon");
  if (icons.length == 0) {
    icons = item.value.querySelectorAll(".icon_flat");
  }
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

/*
onMounted(() => {
});
*/
</script>

<template>
  <div class="item" ref="item" :class="{ alignright: props.alignright }">
    <slot></slot>
  </div>
  <div class="tooltip">
    <div class="tip_message">{{ props.tooltip }}</div>
  </div>
</template>

<style></style>

<style scoped>
.tooltip {
  position: relative;
  top: 40px;
  left: -20px;
  display: none;
  z-index: 100;
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
