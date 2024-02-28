<script setup>
import {ref} from 'vue'
import Portal from "./Portal.vue";
import Popup from "./Popup.vue";
import {useStore} from "./store.js";


const count = ref(0)

const countref = ref(0)

const open = ref(false)

const msg = ref('')

const store = useStore()

function dfdfdf() {
  console.log('test')
  // var childWindow = window.open('', 'modal')
  open.value = !open.value
}

</script>

<template>
  <Portal v-model="open" title="test">
    <Popup :msg="msg" :msg2="store.msg2"></Popup>
  </Portal>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>
  <p ref="countref">
    count is {{ count }}
  </p>
  <input v-model="msg" >

  <input v-model="store.msg2" >
  <p>{{store.msg2}}</p>
  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
    >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Install
    <a href="https://github.com/johnsoncodehk/volar" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
  <button @click="dfdfdf">
    test
  </button>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
