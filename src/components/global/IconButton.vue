<template>
  <div class="icon-button-container">
    <button class="icon-button" v-on:click="methods.clickButton" type="button">
      <FontAwesomeIcon :icon="props.icon" :style="{ color: props.color }" />
    </button>
  </div>
</template>
<script setup lang="ts">

import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

const props = defineProps({
  clickBehavior: Function,
  icon: Array,
  color: String
})
const methods = {
  clickButton(event: MouseEvent) {
    event.stopPropagation()
    props.clickBehavior && props.clickBehavior(event)
  }
}
</script>
<style scoped lang="scss">
@import "@assets/main";

.icon-button-container {
  margin: 0 5px;
  display: inline-block;

  .icon-button {
    background-color: white;
    border: 1px solid $standard-gray-in-white;
    border-radius: 10px;
    transition: $duration;

    svg {
      transition: $duration;
    }

  }
}

@media (prefers-color-scheme: dark) {

  .icon-button-container {

    .icon-button {
      background-color: $dark-mode-panel;
      border: 1px solid $dark-mode-border;
    }
  }
}
</style>
