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
    socketURL: String,
  },
  methods: {
    initTerm() {
      const term = new Terminal({
        cursorBlink: true,
        disableStdin: false,
        convertEol: true
      });
      const fitAddon = new FitAddon();
      var socket = new WebSocket(this.socketURL);
      const attachAddon = new AttachAddon()
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
      this.term = term;
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