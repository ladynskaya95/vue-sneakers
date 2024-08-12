<script setup>
  const emit = defineEmits(["openDrawer"])
</script>


<template>
    <header class="flex justify-between border-b border-slate-200 px-10 py-8">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="logo" class="w-10">
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-400">The biggest selection of sneakers</p>
        </div>
      </div>

      <ul class="flex items-center gap-10">
        <li 
        @click="() => emit('openDrawer')"
        class="flex items-center cursor-pointer gap-3 text-gray-600 hover:text-black">
          <img src="/cart.svg" alt="cart">
          <b>34$</b>
        </li>
      
        <li class="flex items-center cursor-pointer gap-3 text-gray-600 hover:text-black">
          <img src="/heart.svg" alt="heart">
          <b>Favorites</b>
        </li>
      
        <li class="flex items-center cursor-pointer gap-3 text-gray-600 hover:text-black">
          <img src="/profile.svg" alt="profile">
          <b>Profile</b>
        </li>
      </ul>

    </header>
</template>