<script setup lang="ts">

import { onMounted } from 'vue';
import { socket } from "@/socket";
import { ref } from 'vue';
import { type Chat } from "@/lib/types"

function onMessage(msg: string) {
    console.log(`Message : "${msg}"`)
}

const selfId = "2"

const chats = ref<Chat[]>([])

const text = ref("")

function send() {
    console.log(text.value)
    
    socket.emit('chat', {
        id: "0",
        userId: "2",
        userName: "Si Vue",
        message: text.value,
        date: new Date()
    })
    
    text.value = ""
}

onMounted(() => {
    socket.on('message', onMessage)
    
    socket.on('chat', (chat: Chat) => {
        chats.value.push(chat)
    })
})

</script>

<template>
    <div class="bg-red-400 h-full flex flex-col">
        <div class="flex-1 bg-gray-200 p-4 flex flex-col">
            <template v-for="chat in chats">
                <div
                    v-if="chat.userId === selfId"
                    class="bg-emerald-300 my-1 p-3 rounded-lg flex flex-col self-end ml-8"
                >
                    <span class="text-sm text-gray-700">Kamu</span>
                    <span>{{ chat.message }}</span>
                </div>
                <div
                    v-else
                    class="bg-white my-1 p-3 rounded-lg flex flex-col self-start mr-8"
                >
                    <span class="text-sm text-gray-700">{{ chat.userName }}</span>
                    <span>{{ chat.message }}</span>
                </div>
            </template>
        </div>
        <div class="flex">
            <input
                class="flex-1 p-2 outline-none"
                type="text"
                v-model="text"
            />
            
            <button
                @click="send"
                class="bg-blue-500 px-3 text-white"
            >
                Send
            </button>
        </div>
    </div>
</template>
