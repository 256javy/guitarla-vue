
<script setup>

    const props = defineProps({
        guitar: {
            type: Object,
            required: true
        }
    })

    defineEmits(['add-to-cart'])
    

</script>

<template>
    <div  class="col-md-6 col-lg-4 my-4 row align-items-center">
        <div class="col-4">
            <img 
                class="img-fluid" 
                v-bind:src="'/img/' + guitar.imagen + '.jpg'" 
                v-bind:alt="'Guitarra ' + guitar.nombre"
            >
        </div>
        <div class="col-8">
            <h3 class="text-black fs-4 fw-bold text-uppercase">{{guitar.nombre}}</h3>
            <p>{{ guitar.descripcion }}</p>
            <p class="fw-black text-primary fs-3">${{guitar.precio}}</p>
            <button 
                type="button"
                class="btn btn-dark w-100 "
                
                v-on:click="$emit('add-to-cart', guitar)"
                >Agregar al Carrito</button>
        </div>
    </div>
</template>

<style scoped>

</style>
