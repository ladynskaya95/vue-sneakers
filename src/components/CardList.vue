<script setup>
import Card from "./Card.vue"

const onClickAdd = () => {
    alert("Add")
}

</script>

<template>
     <div class="grid grid-cols-4 gap-5">
        <Card 
            title="Men's Sneakers Nike Blazer Mid Suede" imageUrl="/sneakers/sneakers-1.jpg"
            :price="34"
            :isAdded="true"
            :isFavorite="true"
            :onClickAdd="onClickAdd"
            />
        <Card/>
        <Card/>
        <Card/>
     </div>
</template>