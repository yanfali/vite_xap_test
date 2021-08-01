<template>
  <NavBar :configs="tabconfigs" @tab-change-event="updateTabConfigs" />
  <button class="btn btn-primary" @click="count++">
    count is: {{ count }}
  </button>
</template>

<script lang="ts">
import { ref, defineComponent, reactive, toRefs } from "vue";
import NavBar from "./NavBar.vue";
import { TabConfig } from "./NavBar.vue";
export default defineComponent({
  name: "XapClient",
  setup: () => {
    const count = ref(0);
    const tabconfigs = ref([
      { displayText: "Firmware Flashing", hasCount: false },
      { displayText: "Keymap Configuration", hasCount: false },
      { displayText: "Console Output", hasCount: true, count, active: true },
    ] as Array<TabConfig>);

    function updateTabConfigs(index: number) {
      const active = tabconfigs.value.find((config) => config.active === true);
      if (active) {
        active.active = false;
      }
      tabconfigs.value[index].active = true;
    }

    return { count, tabconfigs, updateTabConfigs };
  },
  components: {
    NavBar,
  },
});
</script>
