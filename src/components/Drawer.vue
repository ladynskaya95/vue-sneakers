<script setup>
    import axios from "axios"

    import { inject, ref, computed } from "vue"
    import DrawerHead from "./DrawerHead.vue"
    import CartItemList from "./CartItemList.vue"
    import InfoBlock from "./InfoBlock.vue"

    const props = defineProps({
        totalPrice: Number,
        vatPrice: Number
    })

    const {
        cart,
        closeDrawer
    } = inject("cart")

    const isCreatingOrder = ref(false)
    const orderId = ref(null)

    const createOrder = async () => {
        try{
            isCreatingOrder.value = true
            const { data } = await axios.post("https://06ac99e555756cbc.mokky.dev/orders", {
                items: cart,
                totalPrice: props.totalPrice.value 
  })
            cart.value = []
            orderId.value = data.id
            
            } catch(err) {
            console.log(err)
            } finally {
            isCreatingOrder.value = false
            }
            }

    
    const cartIsEmpty = computed(() => cart.value.length === 0)
    const buttonDisabled = computed(() => 
        isCreatingOrder.value || cartIsEmpty.value)

    
</script>

<template >
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
        <DrawerHead/>

       
        <div v-if="!totalPrice || orderId"
        class="flex h-full items-center">
            <InfoBlock
                v-if="!totalPrice && !orderId"
                title="Cart is empty"
                description="Add at least one pair of sneakers to place an order"
                imageUrl="/package-icon.png"
                />
                <InfoBlock
                v-if="orderId"
                title="Order has been created!"
                :description="`Your order #${orderId} will be transferred to courier delivery soon`"
                imageUrl="/order-success-icon.png"
                />
                
               
        </div>

        <div v-else>
        <CartItemList/>
    
    
        <div class="flex flex-col gap-4 mt-7">
            <div class="flex gap-2">
                <span>Total:</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ totalPrice }}$</b>
            
        </div>

         <div class="flex gap-2">
                <span>Tax 5%:</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{vatPrice}}$</b>
           
        </div>

        <button 
        :disabled="buttonDisabled"
        @click="createOrder"
        class="mt-4 bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 active:bg-lime-800 disabled:bg-slate-300 transition cursor-pointer">
            Checkout
        </button>
    </div>
    </div>
    </div>
</template>