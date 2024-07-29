<script setup>
import Header from "./components/Header.vue"
import CardList from "./components/CardList.vue"
import Drawer from "./components/Drawer.vue"

</script>

<template>
 <!-- <Drawer/> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-15">
    <Header/>

   <div class="p-10">
     <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">All Sneakers</h2>

    <div class="flex gap-4">
        <select class="py-2 px-3 borded rounded-md outline-none">
          <option>By name</option>
          <option>By price (cheap)</option>
          <option>By price (expensive)</option>
        </select>

        <div class="relative">
          <img 
            class="absolute left-4 top-3"
            src="/search.svg" 
            alt="search">
          <input 
            class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            placeholder="Search.."
            type="text"/>
        </div>
    </div>
    </div>


    <CardList/>
     
   </div>
  </div>
</template>

<style scoped>
</style>
