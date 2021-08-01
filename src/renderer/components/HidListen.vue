<template>
  <div>Connected: {{ connects }}, Disconnected: {{ disconnects }}</div>
  <div v-for="(event, index) in hidMessage" :key="index">
    {{ event.timestamp }} {{ event.device.manufacturer }}/{{
      event.device.product
    }}: {{ event.text }}
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, Ref, onMounted } from 'vue';
export default defineComponent({
  name: 'HidListen',
  setup() {
    const connects = ref(0);
    const disconnects = ref(0);
    const hidMessage: Ref<Array<any>> = ref([]);
    onMounted(() => {
      window.ipc.answerMain(
        'hid_listen-connect',
        (event: HidConnectionEvent) => {
          console.log(event);
          connects.value++;
          hidMessage.value.push(event);
        }
      );
      window.ipc.answerMain(
        'hid_listen-disconnect',
        (event: HidConnectionEvent) => {
          console.log(event);
          disconnects.value++;
          hidMessage.value.push(event);
        }
      );
      window.ipc.answerMain('hid_listen-text', (event: HidListenTextEvent) => {
        console.log(event);
        hidMessage.value.push(event);
      });
    });
    return {
      connects,
      disconnects,
      hidMessage,
    };
  },
});
</script>
