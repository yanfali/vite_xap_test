<template>
  <div class="tabs" id="navbar">
    <a class="tab tab-bordered">Firmware Flashing</a>
    <a class="tab tab-bordered">Keymap Configuration</a>
    <a class="tab tab-bordered tab-active"
      >Console Output
      <div class="indicator-item badge">8</div></a
    >
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "NavBar",
});
</script>

<style scoped>
#navbar {
  margin-bottom: 0.6em;
}
</style>
