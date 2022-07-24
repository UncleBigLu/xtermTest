<template>
  <div id="terminal-container"></div>
</template>

<script>
import {Terminal} from 'xterm';
import {FitAddon} from 'xterm-addon-fit';
import {AttachAddon} from 'xterm-addon-attach';
import "xterm/css/xterm.css"


export default {
  name: "MyXterm",
  props:{
    socket_url: String,
  },
  methods: {
    initSocket() {
      var socket = new WebSocket(this.socket_url);
      socket.onopen = () => {
        this.term.write('Websocket opened.\n');
      }
      socket.onclose = () => {
        this.term.write('Websocket closed. Reconnect will be attempted after 2 second.\n');
        setTimeout( () => {
          this.initSocket();
        }, 2000);
      }
      socket.onerror = () => {
        socket.close();
      };
      const attachAddon = new AttachAddon(socket);
      this.term.loadAddon(attachAddon);
    },
    initTerm() {
      const term = new Terminal({
        cursorBlink: true,
        disableStdin: false,
        convertEol: true
      });
      this.term = term;
      const fitAddon = new FitAddon();
      this.initSocket();
      term.loadAddon(fitAddon);

      term.open(document.getElementById('terminal-container'));
      fitAddon.fit();


      term.promt = () => {
        term.write('\r\n$');
      };
      term.onKey(
          ({key, domEvent}) => {
            console.log(key);
            console.log(domEvent);
          }
      );
      term.onData(
          (s) => {
            console.log('Ondata event triggered');
            console.log(s);
          }
      )

      term.write('Hello from \x1B[1;3;31mxterm.js\x1B[0m ');
      term.promt();
    },
  },
  mounted() {
    this.initTerm();
  }
}
</script>

<style scoped>
#terminal-container {
  width: 50vw;
  height: 50vh;
}
</style>