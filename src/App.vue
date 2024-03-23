<script setup>
    import { ref, reactive, onMounted } from 'vue';
    import {db} from './data/guitars.js'
    import Guitar from './components/Guitar.vue'
    import Header from './components/Header.vue'
    import Footer from './components/Footer.vue'

    const guitars = ref([])
    const cart = ref([])

    onMounted(() => {
        guitars.value = db

    })

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
            const toRemove = cart.value.findIndex(guitarInCart => guitarInCart.id === g.id)
            cart.value.splice(toRemove, 1)
        } else {
            g.qty--
        }
    }

</script>

<template>

    <Header 
        v-bind:cart="cart"
        v-on:increase-qty="increaseQuantity"
        v-on:decrease-qty="decreaseQuantity"
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
