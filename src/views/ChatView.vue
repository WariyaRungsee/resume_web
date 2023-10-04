<script setup>
import { onMounted, onUpdated, ref, watch } from 'vue';
import {
    app,
    auth,
    database,
    ref as refDb,
    set,
    push,
    onValue,
    child,
    get
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
    if (chat.value != '' && historykey.value != '') {
        push(refDb(database, `all_chat/${historykey.value}`), {
            "user": studentId,
            "message": chat.value,
            "dateTime": new Date().toISOString()
        });
        chat.value = "";
    }
}

onValue(db, (snapshot) => {
    const data = snapshot.val () ?? [];
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
let groupChatName = ref("");
const createGroup = () =>{
    if (groupChatName.value != ''){
        get(child(db, `${groupChatName.value}`)).then((snapshot) =>{
            if(snapshot.exists()){
                alert("Cannot create chat because chat is exists.")
            } else {
                push(refDb(database, `all_chat/${groupChatName.value}`),{
                     "user": studentId,
                     "message": '',
                     "dateTime": new Date().toISOString()
        });
       
        }
        groupChatName.value='';
        }).catch((err) => {
            console.error(err);
        })
   
    }
}

const deleteGroup = (key) => {
    // ตรวจสอบว่า key ไม่เป็นค่าว่าง
    if (key !== '') {
        // ลบกลุ่มที่ถูกเลือกออกจาก Firebase Realtime Database
        const groupRef = refDb(database, `all_chat/${key}`);
        set(groupRef, null); // ลบข้อมูลทั้งกลุ่ม

        // เลือกกลุ่มใหม่หลังการลบ
        // if (historykey.value === key) {
        //     historykey.value = '';
        // }
    }
};

</script>

<template>
    <div class="flex">
        <div class=" bg-[#F5F5F5] h-[90vh] w-[30%]">
            <div class="overflow-y-scroll w-full h-[90%]">
                <div class="card card-side bg-base-100 shadow-xl w-full mb-4" style="cursor: pointer;"
                    v-for="(group, index) in histories" :key="index" @click="$event => selectGroup(index)">

                    <div class="card-body">
 
                        <h2 class="card-title">{{ index }}</h2>
                        <p>{{ group[Object.keys(group)[Object.keys(group).length - 1]]?.message }}</p> 
                        <!-- <button class="btn btn-square btn-error" @click="deleteGroup(index)">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
                        </button>
                        <button @click="deleteGroup(index)" class="btn rounded-full">Delete</button>  -->

                        
                        <div style="display: flex; justify-content: flex-end;">
                        <button class="btn btn-square btn-error" @click="deleteGroup(index)">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
                        </button>
                        </div> 

                    </div>
                </div>

            </div>
            <div class="w-full h-[10%] pt-4">
                <button  class="btn w-full h-full" data-theme="business" onclick="my_modal_1.showModal()">Add Group</button>
                <dialog id="my_modal_1" class="modal">
                    <div class="modal-box">
                        <div class="from-control w-full">
                            <label class="label">
                                <span class="label-text">Group Name</span>
                            </label>
                            <input type="text" placeholder="Type here" class="input input-bordered w-full" v-model="groupChatName"/>
                        </div>
               

                        <div class="modal-action">
                            <button class="btn btn-warning" @click="createGroup">create Group</button>
                            <form method="dialog">
                                <!-- if there is a button in form, it will close the modal -->
                                <button class="btn btn-error">Close</button>
                            </form>
                        </div>
                    </div>
                </dialog>
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
                <button @click="onSend" class="w-[20%] btn btn-info">send</button>

            </div>
        </div>
    </div>
</template>
<style scoped></style>
