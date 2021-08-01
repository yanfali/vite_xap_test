<template>
  <NavBar />
  <button class="btn btn-primary" @click="count++">
    count is: {{ count }}
  </button>
</template>

<script lang="ts">
import { ref, defineComponent } from "vue";
import NavBar from "./NavBar.vue";
export default defineComponent({
  name: "XapClient",
  setup: () => {
    const count = ref(0);
    return { count };
  },
  components: {
    NavBar,
  },
});
</script>
