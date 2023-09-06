<script setup>
import { onMounted,ref } from "vue";
import axios from "axios";
import { useRouter } from 'vue-router'
let name = ref(''),
    price = ref(0),
    routerName = ref(''),
    _id = ref('');
const router = useRouter()
onMounted(() => {
  routerName.value = router.currentRoute.value.name;
  _id.value = router.currentRoute.value.params?.id;
  console.log("routerName", routerName.value);
  console.log("_id",_id.value);
  if(routerName.value == "edit"){
    initEdit();
  }
});
const API_PATH = import.meta.env.VITE_API_PATH;
const initEdit = async () => {
  const {data} =await axios.get(`${API_PATH}/getproduct/${_id.value}`);
  name.value = data.data.name;
  price.value = data.data.price;
}
const Submit = async () => {
  const req ={
    name:name.value, price:price.value
  };
  if(routerName.value == "create") {
    await axios.post(
      `${API_PATH}/create`,
      req
    ).then((response) => {
      name.value ='';
      price.value = '';
    });
  }else {
    await axios.post(
      `${API_PATH}/update/${_id.value}`,
      req
    ).then((response) => {
      router.push({name : 'home', replace: true});

    });
  }
}

const submit = async () => {
 
}
</script>

<template>
  <div class="section" data-theme="winter">
    <div class="m-auto">
      <div class="flex justify-center px-4 py-5">
        <label for="" class="m-3">Name :</label>
        <input
          type="text"
          placeholder=" "
          class="input input-bordered input-info w-full max-w-xs"
          v-model="name"
        />

        <label for="" class="m-3">Price :</label>
        <input
          type="text"
          placeholder=" "
          class="input input-bordered input-error w-full max-w-xs"
          v-model="price"
        />
      </div>
      <div class="text-center m-3">
        <button class="btn btn-outline btn-success" v-on:click="Submit()">Submit</button>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
