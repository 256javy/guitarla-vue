
<script setup>
    import { computed } from 'vue';

    const props = defineProps({
        cart: {
            type: Array,
            required: true
        },
        mainGuitar: {
            type: Object,
            required: true
        }
    })

    defineEmits([
        'increase-qty',
        'decrease-qty',
        'add-to-cart',
        'remove-from-cart',
        'clear-cart'
    ])

    const totalToPay = computed( () => {
        return props.cart.reduce( (total, guitar) => total + (guitar.qty * guitar.precio), 0)
    })

</script>

<template>
    <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid" src="/img/logo.svg" alt=" logo">
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div 
                        class="carrito"
                    >
                        <img class="img-fluid" src="/img/carrito.png" alt=" carrito" />

                        <div id="carrito" class="bg-white p-3">
                            <p v-if="cart.length === 0" class="text-center">El carrito esta vacio</p>
                            <div v-else class="">
                                <table  class="w-100 table">
                                    <thead>
                                        <tr>
                                            <th>Imagen</th>
                                            <th>Nombre</th>
                                            <th>Precio</th>
                                            <th>Cantidad</th>
                                            <th></th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr
                                            v-for="guitar in cart"
                                        >
                                            <td>
                                                <img 
                                                class="img-fluid"
                                                v-bind:src="'/img/' + guitar.imagen + '.jpg'" 
                                                v-bind:alt="'Guitarra ' + guitar.nombre">
                                            </td>
                                            <td>{{guitar.nombre}}</td>
                                            <td class="fw-bold">
                                                    ${{ guitar.precio }}
                                            </td>
                                            <td class="flex align-items-start gap-4">
                                                <button
                                                    type="button"
                                                    class="btn btn-dark"
                                                    v-on:click="$emit('decrease-qty', guitar)"
                                                >
                                                    -
                                                </button>
                                                    {{guitar.qty}}
                                                <button
                                                    type="button"
                                                    class="btn btn-dark"
                                                    v-on:click="$emit('increase-qty', guitar)"
                                                >
                                                    +
                                                </button>
                                            </td>
                                            <td>
                                                <button
                                                    class="btn btn-danger"
                                                    type="button"
                                                    v-on:click="$emit('remove-from-cart', guitar.id)"
                                                >
                                                    X
                                                </button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                                <p class="text-end">Total pagar: <span class="fw-bold">${{totalToPay}}</span></p>
                                <button 
                                    class="btn btn-dark w-100 mt-3 p-2"
                                    v-on:click="$emit('clear-cart')"
                                    >Vaciar Carrito</button>
                            </div>
                        </div>
                    </div>
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">Modelo {{ mainGuitar.nombre }}</h1>
                    <p class="mt-5 fs-5 text-white">{{ mainGuitar.descripcion }}</p>
                    <p class="text-primary fs-1 fw-black">${{mainGuitar.precio}}</p>
                    <button 
                        type="button"
                        class="btn fs-4 bg-primary text-white py-2 px-5"
                        v-on:click="$emit('add-to-cart', mainGuitar)"
                    >Agregar al Carrito</button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="/img/header_guitarra.png" alt=" header">
    </header>
</template>
