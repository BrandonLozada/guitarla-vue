<script setup>
import { ref, reactive, onMounted } from 'vue'
import { db } from './data/guitarras'
import Guitarra from './components/Guitarra.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

const guitarras = ref([])
const carrito = ref([])
const guitarra = ref({})

/*
    Consumo de guitarras
*/
onMounted(() => {
  guitarras.value = db
  guitarra.value = db[3]
})

/*
    Evento de agregarCarrito
*/
const agregarCarrito = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)

    if (existeCarrito >= 0) {
        carrito.value[existeCarrito].cantidad++
    } else {
        guitarra.cantidad = 1;
        carrito.value.push(guitarra)
    }
}

/*
    Modificar cantidades en carrito
*/
const decrementarCantidad = (id) => {
  const index = carrito.value.findIndex(producto => producto.id === id)
  if (carrito.value[index].cantidad <= 1) return
  carrito.value[index].cantidad--
}

const incrementarCantidad = (id) => {
  const index = carrito.value.findIndex(producto => producto.id === id)
  if (carrito.value[index].cantidad >= 5) return
  carrito.value[index].cantidad++
}
</script>

<template>
    <Header
        :carrito="carrito"
        :guitarra="guitarra"
        @decrementar-cantidad="decrementarCantidad"
        @incrementar-cantidad="incrementarCantidad"
        @agregar-carrito="agregarCarrito"
    >
    </Header>
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <!-- 
                Renderizado de componente Guitarra 
            -->
            <Guitarra
              v-for="guitarra in guitarras"
              v-bind:guitarra="guitarra"
              @agregar-carrito="agregarCarrito"
            >
            </Guitarra>

        </div>
    </main>

    <Footer></Footer>

</template>
