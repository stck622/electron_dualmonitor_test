<script setup>

import {useStore} from "./store.js";

const props = defineProps(['msg', 'msg2'])

const store = useStore()
</script>


<template>
  <h1>{{ props.msg }}</h1>
  <h1>{{ props.msg2 }}</h1>
  <h1>{{ store.msg2 }}</h1>
</template>


<style scoped>

</style>
