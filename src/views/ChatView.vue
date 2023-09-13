<script setup>
import { onMounted ,ref} from "vue";
import {
  app,
  auth,
  database,
  ref as refDb,
  set,
  push
} from '../firebaseConfig'
let chat = ref("");
const studentId = "6314110013"
const onSend = () => {
    push(refDb(database, 'all_chat/group_/1'),{
        "user":studentId,
        "message": chat.value,
        "dateTime": new Date().toISOString()
    });
    chat.value ="";
    
}

// onMounted(() => {
//     push(refDb(database,'test'), {
//     "6314110013": "Hello World"
//   });
// });
</script>

<template>
  <div class="flex">
    <div class="overflow-y-scroll bg-white h-[90vh] w-[30%]">
      <div
        class="w-full bg-[#EDD7FA] h-36 border-pink-400 border-4"
        v-for="i in 100" :key="i"
      ></div>
      <!-- <p v-for="i in 100" :key="i">test</p> -->
    </div>

    <div class="bg-gray-50 h-[90vh] w-[70%]">
      <div class="overflow-y-scroll h-[90%]">
        <!-- <p v-for="i in 100" :key="i">test</p> -->
        <div class="chat chat-start">
          <div class="chat-bubble chat-bubble-info">It's over Anakin, <br />I have the high ground.
          </div>
        </div>
        <div class="chat chat-end">
          <div class="chat-bubble chat-bubble-info">You underestimate my power!</div>
        </div>
      </div>
      <div class="flex h-[10%] gap-4">
        <input v-on:keyup.enter="onSend"  v-model="chat" type="text" placeholder="Typer here" class="input input-bordered w-[80%] h-full"/>
        <button @click="onSend" class="w-[20%] btn h-full">send</button>
      </div>
    </div>
  </div>
</template>
<style scoped></style>
