<script setup>
import Card from "./Card.vue"
</script>

<template>
     <div class="grid grid-cols-4 gap-5">
        <Card 
            title="Men's Sneakers Nike Blazer Mid Suede" imageUrl="/sneakers/sneakers-1.jpg"
            :price="34"
            />
        <Card/>
        <Card/>
        <Card/>
     </div>
</template>