<script setup>

import {ref, onMounted, onBeforeUnmount, watch, nextTick} from "vue";

const props = defineProps(
  {
    modelValue: Boolean,
    title: String,
    width: {
      type: Number,
      default: 600
    },
    height: {
      type: Number,
      default: 400
    }
  }
);

const emit = defineEmits(["update:modelValue"]);

let windowRef = null
const portal = ref(null);

const copyStyles = (sourceDoc, targetDoc) => {

  // eslint-disable-next-line unicorn/prefer-spread
  for(const styleSheet of Array.from(sourceDoc.styleSheets)) {
    if(styleSheet.cssRules) {
      // for <style> elements
      const nwStyleElement = sourceDoc.createElement("style");

      // eslint-disable-next-line unicorn/prefer-spread
      for(const cssRule of Array.from(styleSheet.cssRules)) {
        // write the text of each rule into the body of the style element
        nwStyleElement.append(sourceDoc.createTextNode(cssRule.cssText));
      }

      targetDoc.head.append(nwStyleElement);
    }
    else if(styleSheet.href) {
      // for <link> elements loading CSS from a URL
      const nwLinkElement = sourceDoc.createElement("link");

      nwLinkElement.rel = "stylesheet";
      nwLinkElement.href = styleSheet.href;
      targetDoc.head.append(nwLinkElement);
    }
  }
};

const openPortal = () => {

  nextTick().then(() => {

    windowRef = window.open("", "", "width=600,height=400,left=200,top=200");
    if(!windowRef || !portal.value) return;
    windowRef.document.body.append(portal.value);
    copyStyles(window.document, windowRef.document);
    windowRef.addEventListener("beforeunload", closePortal);
  })
    .catch((error) => console.error("Cannot instantiate portal", error.message));
};

const closePortal = () => {
  if(windowRef) {
    windowRef.close();
    windowRef = null;
    emit("update:modelValue", false);
  }
};

watch(props, () => {
  if(props.modelValue) {
    openPortal();
  }
  else {
    closePortal();
  }
});

onMounted(() => {
  if(props.modelValue) {
    openPortal();
  }
});
onBeforeUnmount(() => {
  if(windowRef) {
    closePortal();
  }
});
</script>

<template>
  <div v-if="props.modelValue" ref="portal">
    <slot />
  </div>
</template>
