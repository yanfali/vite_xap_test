<template>
  <div>Connected: {{ connects }}, Disconnected: {{ disconnects }}</div>
  <div ref="terminal" id="terminal"></div>
</template>

<script lang="ts">
import { defineComponent, ref, Ref, onMounted } from 'vue';
import { ITerminalOptions, Terminal } from 'xterm';
import 'xterm/css/xterm.css';
export default defineComponent({
  name: 'HidListen',
  setup() {
    const connects = ref(0);
    const disconnects = ref(0);
    const hidMessage: Ref<Array<any>> = ref([]);
    const term = new Terminal({
      cols: 90,
      scrollback: 500,
    } as ITerminalOptions);
    onMounted(() => {
      console.log(window.ipc);
      window.ipc.answerMain(
        'hid_listen-connect',
        (event: HidConnectionEvent) => {
          console.log(event);
          connects.value++;
          term.write(
            `${event.timestamp} ${event.device.manufacturer} ${event.device.product}\r\n`
          );
        }
      );
      window.ipc.answerMain(
        'hid_listen-disconnect',
        (event: HidConnectionEvent) => {
          console.log(event);
          disconnects.value++;
          term.write(
            `${event.timestamp} ${event.device.manufacturer} ${event.device.product}\r\n`
          );
        }
      );
      window.ipc.answerMain('hid_listen-text', (event: HidListenTextEvent) => {
        console.log(event);
        term.write(
          `${event.timestamp} ${event.device.manufacturer} ${event.device.product}: ${event.text}\r\n`
        );
      });
      const terminal = document.getElementById('terminal');
      if (terminal !== null) {
        term.open(terminal);
        term.write('Hello, World!\r\n');
      }
    });
    return {
      connects,
      disconnects,
      hidMessage,
    };
  },
});
</script>
