<script setup lang="ts">
import { ref, onMounted, useTemplateRef } from "vue";

const props = defineProps(["tooltip", "name", "isClearable", "placeholder","alignright"]);

const emit = defineEmits(["toolbarItemSelect"]);

const item = useTemplateRef("item");

onMounted(() => {
  const items = item.value.querySelectorAll(".selectitem");
  items.forEach((icon) => {
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
    icon.addEventListener("mousedown", function () {
      const targetIcon = this;
      const activeTooltip = targetIcon.parentElement.nextElementSibling;
      activeTooltip.classList.remove("tip_active");
    });
    //icon.addEventListener("mousedown", item_mousedown) ;
    //icon.addEventListener("mouseup", item_mouseup);
    /*
       icon.addEventListener("mousedown", function (e) {
           const targetIcon = this
           //const icon = targetIcon.parentElement
	   if (targetIcon.classList.contains("toggle")) {
              //console.log("TOGGLE DOWN");
              targetIcon.classList.toggle("mouse-down")
	      if (targetIcon.classList.contains("mouse-down")) {
                  item_toggle_switch(e, true);

	      } else {
                  item_toggle_switch(e, false);

	      }
	      return;
	   }
           targetIcon.classList.add("mouse-down")
           item_mousedown(e);
       });
       icon.addEventListener("mouseup", function (e) {
           const targetIcon = this
           //const icon = targetIcon.parentElement
	   if (targetIcon.classList.contains("toggle")) {
              //console.log("TOGGLE UP");
	      return;
	   }
           targetIcon.classList.remove("mouse-down")
           item_mouseup(e);
       });
 */
  });
});
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
  z-index: 100;
}

.tip_active {
  display: block;
}

.alignright {
  margin-left: auto;
}
</style>
