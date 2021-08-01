<template>
  <div class="tabs" id="navbar">
    <a v-for="(tab, index) in tabs" :key="index" class="tab tab-bordered"
      >{{ tab.displayText }}
      <div v-if="tab.hasCount" class="indicator-item badge">
        {{ tab.count }}
      </div>
    </a>
  </div>
</template>
<script lang="ts">
export interface TabConfig {
  displayText: string;
  hasCount: boolean;
  count?: number;
  active?: boolean;
}
import { ref, defineComponent } from "vue";
export default defineComponent({
  name: "NavBar",
  props: {
    configs: {
      type: Array,
      default: [],
    },
  },
  setup(prop) {
    const tabs: Array<TabConfig> = prop.configs as Array<TabConfig>;
    return { tabs };
  },
});
</script>

<style scoped>
#navbar {
  margin-bottom: 0.6em;
}
</style>
