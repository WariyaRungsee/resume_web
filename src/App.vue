<!-- <template>
  <div class="section" data-theme="valentine">
    <div class="flex h-screen">
      <div class="m-auto">

        <div class="avatar">

          <div class="w-90 rounded">
            <img src="./assets/Images/Image.png" />
          </div>
        </div>

         <div class="mockup-window border bg-base-300">
          <div class="flex justify-center px-4 py-16 bg-base-200"> Name : Wariya Rungsee</div>
        </div> 
      </div>
    </div>
  </div>
  <div class="section" data-theme="winter">
    <div class="flex h-screen">
      <div class="m-auto">
        <h3 class="text-9xl">Resume</h3>
        <progress class="progress w-100"></progress>
        <div class="mockup-window border bg-base-300">
          <div class="chat chat-start">
            <div class="chat-bubble">What is your name ?</div>
          </div>
          <div class="chat chat-end">
            <div class="chat-image avatar">
              <div class="w-10 rounded-full">
                <img src="./assets/Images/Image.png" />
              </div>
            </div>
            <div class="chat-header">
              Wariya
              <time class="text-xs opacity-50">12:46</time>
            </div>
            <div class="chat-bubble">My name is Wariya Rungsee</div>
            <div class="chat-footer opacity-50">
              Seen at 12:46
            </div>
          </div>

          <div class="chat chat-start">
            <div class="chat-bubble">Hom old are you ?</div>
          </div>

          <div class="chat chat-end">
            <div class="chat-image avatar">
              <div class="w-10 rounded-full">
                <img src="./assets/Images/Image.png" />
              </div>
            </div>
            <div class="chat-header">
              Wariya
              <time class="text-xs opacity-50">12:47</time>
            </div>
            <div class="chat-bubble">I'm 21 years old</div>
            <div class="chat-footer opacity-50">
              Seen at 12:47
            </div>
          </div>

          <div class="chat chat-start">
            <div class="chat-bubble">What is your birthday ?</div>
          </div>

          <div class="chat chat-end">
            <div class="chat-image avatar">
              <div class="w-10 rounded-full">
                <img src="./assets/Images/Image.png" />
              </div>
            </div>
            <div class="chat-header">
              Wariya
              <time class="text-xs opacity-50">12:48</time>
            </div>
            <div class="chat-bubble">My Birthday is 28 November</div>
            <div class="chat-footer opacity-50">
              Seen at 12:48
            </div>
          </div>


          <div class="chat chat-start">
            <div class="chat-bubble">What is your mobile phone number ?</div>
          </div>

          <div class="chat chat-end">
            <div class="chat-image avatar">
              <div class="w-10 rounded-full">
                <img src="./assets/Images/Image.png" />
              </div>
            </div>
            <div class="chat-header">
              Wariya
              <time class="text-xs opacity-50">12:49</time>
            </div>
            <div class="chat-bubble">095XXXXXXX</div>
            <div class="chat-footer opacity-50">
              Seen at 12:49
            </div>
          </div>

          <div class="chat chat-start">
            <div class="chat-bubble">Where do you study ?</div>
          </div>

          <div class="chat chat-end">
            <div class="chat-image avatar">
              <div class="w-10 rounded-full">
                <img src="./assets/Images/Image.png" />
              </div>
            </div>
            <div class="chat-header">
              Wariya
              <time class="text-xs opacity-50">12:50</time>
            </div>
            <div class="chat-bubble">I study at MUT</div>
            <div class="chat-footer opacity-50">
              Seen at 12:50
            </div>
          </div>
        </div>
        <progress class="progress w-100"></progress>
         <div class="mockup-window border bg-base-300">
          <div class="flex justify-center px-4 py-16 bg-base-200">Wariya Rungsee</div>
        </div> 
      </div>
    </div>
  </div>

  <div class="section" data-theme="halloween">
    <table class="table">
      <thead>
        <tr>
          <td>ID</td>
          <td>Product name</td>
          <td>Product price</td>
        </tr>
      </thead>
      <tbody>
        <tr class="hover" v-for="(product, index) in products" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ product.name }}</td>
          <td>{{ product.price }}</td>

        </tr>

      </tbody>

    </table>

  </div>
</template> -->

<template>
  <header>
    
    <div class="navbar bg-base-100">
      <div class="navbar-start">
        <div class="dropdown">
          <label tabindex="0" class="btn btn-ghost lg:hidden">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h8m-8 6h16" /></svg>
          </label>
          <ul tabindex="0" class="menu menu-sm dropdown-content mt-3 z-[1] p-2 shadow bg-base-100 rounded-box w-52">
            <li v-for="menu in menuList" :key="menu.name"><RouterLink :to="menu.path">{{ menu.name }}</RouterLink></li>
          </ul>
        </div>
        <a class="btn btn-ghost normal-case text-xl">daisyUI</a>
      </div>
      <div class="navbar-center hidden lg:flex">
        <ul class="menu menu-horizontal px-1">
          <li v-for="menu in menuList" :key="menu.name"><RouterLink :to="menu.path">{{ menu.name }}</RouterLink></li>
        </ul>
      </div>
      <!-- <div class="navbar-end">
        <a class="btn">Button</a>
      </div> -->
    </div>
  </header>
  <div class="container mx-auto">
    <RouterView :key="router.currentRoute.value.name"/>
  </div>   
</template>

<script setup>
import { RouterView, RouterLink } from 'vue-router';
import router from './router';
const menuList = [
  { path:"/", name: "Home"},
  { path:"/create", name: "Create"},
  { path:"/chat", name: "Chat"}
]

</script>

<style scoped>

</style>



<!-- <script setup>
import axios from 'axios';
// import axiosfrom  from 'axios'
import { ref, onMounted } from 'vue';
let products = ref()
onMounted(async () => {
  products.value = await getAllProduct();
  console.log(products);
})

const getAllProduct = async () => {
  let data = [];
  data = await axios.get('http://localhost:5000/getallproduct').then((response) => {
    return response.data;
  });
  return data;
}
</script>

<style scoped>
.section {
  height: 100vh;
}
</style> -->