<script setup lang="ts">

import { onMounted } from 'vue';
import { socket } from "@/socket";

function onMessage(msg: string) {
    console.log(`Message : "${msg}"`)
}

onMounted(() => {
    socket.on('message', onMessage)
})

</script>

<template>
  <main>
    <div>HOME</div>
  </main>
</template>
../lib/socket.js