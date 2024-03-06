<script setup>
    import Header from './components/Header.vue'
    import Footer from './components/Footer.vue'
    import { ref, reactive, onMounted, watch } from 'vue'
    import {db} from './data/guitarras'
    import Guitarra from './components/Guitarra.vue'

    const guitarras = ref([])
    const carrito = ref([])
    const guitarra = ref({})

    watch(carrito,()=>{
        guardarLocalStorage();
    },{
        deep:true
    })

    onMounted(() => {
        guitarras.value = db;
        guitarra.value = db[3];

        const carritoStorage = localStorage.getItem('carrito')
        if (carritoStorage){
            carrito.value = JSON.parse(carritoStorage)
        }
    })
    const guardarLocalStorage = () => {
        localStorage.setItem('carrito', JSON.stringify(carrito.value))
    }
    const agregarCarrito = (guitarra) => {
        const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)
        if(existeCarrito >= 0){
            carrito.value[existeCarrito].cantidad++
        }else{
            guitarra.cantidad=1;
            carrito.value.push(guitarra)
        }
    }
    const decrementarCantidad  = (id) => {
        const index = carrito.value.findIndex(producto => producto.id === id)
        if (carrito.value[index].cantida <= 1) return
        carrito.value[index].cantidad--
    }
    const incrementarCantidad  = (id) => {
        const index = carrito.value.findIndex(producto => producto.id === id)
        if (carrito.value[index].cantidad >= 5) return
        carrito.value[index].cantidad++
    }
    const eliminarProducto = (id) => {
        carrito .value= carrito.value.filter(producto => producto.id !== id)

    }
    const vaciarCarrito = (id) => {
        carrito.value=[];
    }
</script>
<template>
    <Header
        :carrito = "carrito"
        :guitarra = "guitarra"
        @agregarCarrito = "agregarCarrito"
        @incrementarCantidad = "incrementarCantidad"
        @decrementarCantidad = "decrementarCantidad"
        @eliminarProducto = "eliminarProducto"
        @vaciarCarrito = "vaciarCarrito"
    />
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>
        <Guitarra
        v-for="guitarra in guitarras"
        :guitarra = "guitarra"
        @agregarCarrito = "agregarCarrito"
        />
        <div class="row mt-5">
            
        </div>
    </main>
    <Footer/>


    
</template>



<style lang="scss" scoped>

</style>