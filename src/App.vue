<script setup>
    import { ref, onMounted, watch } from 'vue';
    import {db} from './data/guitars.js'
    import Guitar from './components/Guitar.vue'
    import Header from './components/Header.vue'
    import Footer from './components/Footer.vue'

    const guitars = ref([])
    const cart = ref([])
    const mainGuitar = ref([])

    watch(cart, () => {
        saveInLocalStorage()
    },{
        deep: true
    })

    onMounted(() => {
        guitars.value = db
        mainGuitar.value = guitars.value[3]

        const cartFromLocalStorage = localStorage.getItem('cart')
        if(cartFromLocalStorage){
            cart.value = JSON.parse(cartFromLocalStorage)
        }

    })

    const saveInLocalStorage = () => {
        localStorage.setItem('cart', JSON.stringify(cart.value))
    }

    const addToCart = (guitar) => {
        const exist = cart.value.findIndex(guitarInCart => guitarInCart.id === guitar.id)
        if (exist < 0) {
            guitar.qty = 1
            cart.value.push(guitar)
        } else {
            cart.value.at(exist).qty++
        }
    }

    const increaseQuantity = (g) => {
        g.qty++
    }

    const decreaseQuantity = (g) => {
        const current = g.qty
        if(current === 1){
            return
        } else {
            g.qty--
        }
    }

    const removeFromCart = (guitarId) => {
        cart.value = cart.value.filter(item => item.id !== guitarId)
    }

    const clearCart = () => {
        cart.value = []
    }

</script>

<template>

    <Header 
        v-bind:cart="cart"
        v-bind:main-guitar="mainGuitar"
        v-on:increase-qty="increaseQuantity"
        v-on:decrease-qty="decreaseQuantity"
        v-on:add-to-cart="addToCart"
        v-on:remove-from-cart="removeFromCart"
        v-on:clear-cart="clearCart"
    />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <Guitar 
                v-for="guitar in guitars"
                v-bind:guitar="guitar"
                v-on:add-to-cart="addToCart"
            />
        </div>
    </main>

    <Footer
    />

</template>
