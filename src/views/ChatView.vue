<script setup>
import { onMounted, onUpdated, ref, watch } from 'vue';
import {
    app,
    auth,
    database,
    ref as refDb,
    set,
    push,
    onValue
} from '../firebaseconfig'
// import { DataSnapshot } from '@firebase/database';

let chat = ref("");
let histories = ref([]);
let historykey = ref(""); //collect select group
const bottomEl = ref(null);
const studentId = "6314110013"
let refChat;
const db = refDb(database, 'all_chat/')

const onSend = () => {
    if (chat.value != '' && historykey.value !=''){
        push(refDb(database, `all_chat/${historykey.value}`), {
        "user": studentId,
        "message": chat.value,
        "dateTime": new Date().toISOString()
    });
    chat.value = "";

    }
    

}

onValue(db, (snapshot) => {
    const data = snapshot.val();
    histories.value = data;
})
// watch(histories, (newHistories, oldHistories) => {
//     bottonEl.value.scrollIntoView({ behavior: 'smooth' });
// });

onUpdated(() => {
    bottomEl.value.scrollIntoView({ behavior: 'smooth' });
});

const selectGroup = (key) => {
    historykey.value = key;
}


// onMounted(() => {
//     push(refDb(database,'test'), {
//     "6314110020": "Hello World"
//   });
// });

</script>
 

<template>
    <div class="flex">
        <div class="overflow-y-scroll bg-[#F5F5F5] h-[90vh] w-[30%]">
            
                <div class="card card-side bg-base-100 shadow-xl w-full mb-4"
                style="cursor: pointer;"
                v-for="(group, index) in histories"
                :key="index"
                @click="$event => selectGroup(index)">
               
            <div class="card-body">
                
                <h2 class="card-title">{{ index }}</h2>
                <p>{{ group[Object.keys(group)[Object.keys(group).length -1]].message }}</p>
            </div>
        </div>
        </div>
        <div class=" bg-gray-50 h-[90vh] w-[70%] ">
            <div class="overflow-y-scroll h-[90%]">
                
                <div v-for="(history, index) in histories[historykey]"
                    :class="`chat ${history.user == studentId ? 'chat-end' : 'chat-start'}`" :key="index">
                    
                    <div class="chat-header">{{ history.user }}
                        <time class="text-xs opacity-50">{{ history.dateTime }}</time>
                    </div>
                    <div class="chat-bubble">{{ history.message }}</div>
                </div>

                <div ref="bottomEl"></div>
            </div>

            <div class="flex h-[10%] gap-4">
                <input v-on:keyup.enter="onSend" v-model="chat" type="text" placeholder="Type here"
                    class="input input-bordered w-[80%] h-full" />
                <!-- <button class="w-[20%] btn h-full"  v-on:click="onSend()" >send</button> -->
                <button @click="onSend" class="w-[20%] btn h-full">send</button>

            </div>
        </div>
    </div>
</template>
<style scoped></style>
