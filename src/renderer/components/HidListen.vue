<template>
  <div v-for="(entry, index) in consoleEntries" :key="index">
    {{ entry }}
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, Ref, onMounted } from 'vue';
export default defineComponent({
  name: 'HidListen',
  setup() {
    const consoleEntries: Ref<Array<any>> = ref([]);
    onMounted(() => {
      function limitLinesTo(count: number) {
        while (consoleEntries.value.length > count) {
          consoleEntries.value.shift();
        }
      }
      function push(text: string) {
        console.log(text);
        consoleEntries.value.push(text);
        limitLinesTo(10);
      }

      window.ipc.answerMain(
        'hid_listen-connect',
        (event: HidConnectionEvent) => {
          console.log(event);
          push(
            `${event.device.manufacturer} ${event.device.product}: connected`
          );
        }
      );
      window.ipc.answerMain(
        'hid_listen-disconnect',
        (event: HidConnectionEvent) => {
          console.log(event);
          push(
            `${event.device.manufacturer} ${event.device.product}: disconnected`
          );
        }
      );
      window.ipc.answerMain('hid_listen-text', (event: HidListenTextEvent) => {
        console.log(event);
        push(
          `${event.device.manufacturer} ${event.device.product}: ${event.text}`
        );
      });
    });
    return {
      consoleEntries,
    };
  },
});
</script>
