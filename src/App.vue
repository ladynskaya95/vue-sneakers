<script setup>

import { onMounted, ref, reactive, watch, provide } from "vue"
import axios from "axios"

import Header from "./components/Header.vue"
import CardList from "./components/CardList.vue"
import Drawer from "./components/Drawer.vue"

const items = ref([])
const cart = ref([])

const drawerOpen = ref(false)

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const filters = reactive({
  sortBy: "title",
  searchQuery: "",
})

const addToCart = (item) => {
  cart.value.push(item)
}

const onChangeSelect = event => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = event => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
    try {
   
    const {data: favorites} = await axios.get("https://06ac99e555756cbc.mokky.dev/favorites")
    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.parentId === item.id);

      if (!favorite) {
        return item;
      }

      return {
       ...item,
        isFavorite: true,
        favoriteId: favorite.id
      };
    })
  }
  catch(err) {
    console.log(err)
  }
}

const addToFavorite = async (item) => {
 try{
  if(!item.isFavorite) {
    const obj = {
      parentId: item.id
    }

    item.isFavorite = true;

    const {data} = await axios.post("https://06ac99e555756cbc.mokky.dev/favorires", obj)

    item.favoriteId = data.id;
 } else {
    item.isFavorite = false;
   await axios.delete(`https://06ac99e555756cbc.mokky.dev/favorires/${item.favoriteId}`)

   
   item.favoriteId = null;
 }
} catch(err) {
   console.log(err)
 
}}


const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
       }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }   

    const {data} = await axios.get("https://06ac99e555756cbc.mokky.dev/items", {
      params
    })
    items.value = data.map(obj => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  }
  catch(err) {
    console.log(err)
  }
}

onMounted(async() => {
  await fetchItems()
  await fetchFavorites()
})

watch(filters, fetchItems)

provide("cartActions", {
  closeDrawer,
  openDrawer
})

</script>

<template>
 <Drawer v-if="drawerOpen"/>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-15">
    <Header @openDrawer="openDrawer"/>
     <div class="p-10">
     <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">All Sneakers</h2>

    <div class="flex gap-4">
        <select 
          @change="onChangeSelect"
          class="py-2 px-3 borded rounded-md outline-none">
          <option value="name">By name</option>
          <option value="price">By price (cheap)</option>
          <option value="-price">By price (expensive)</option>
        </select>

        <div class="relative">
          <img 
            class="absolute left-4 top-3"
            src="/search.svg" 
            alt="search">
          <input 
            @input="onChangeSearchInput"
            class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            placeholder="Search.."
            type="text"/>
        </div>
    </div>
    </div>
    <div class="mt-10">
      <CardList :items="items"
      @addToFavorite="addToFavorite" 
      @addToCart="addToCart"
      />
    </div>
   </div>
  </div>

</template>
<style scoped>
</style>
