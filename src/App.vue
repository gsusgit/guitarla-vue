<script setup>
import {ref, onMounted, watch} from 'vue'
import {db} from './data/guitarras'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import Guitarra from './components/Guitarra.vue'

const guitarras = ref([])
const guitarra = ref({})
const carrito = ref([])

watch(carrito,() => {
  guardarLocalStorage()
}, {
  deep: true
})

const agregarCarrito = (guitarra) => {
  const existeProducto = carrito.value.findIndex(producto => producto.id === guitarra.id)
  if (existeProducto >= 0) {
    carrito.value[existeProducto].cantidad++
  } else {
    guitarra.cantidad = 1
    carrito.value.push(guitarra)
  }
}

const decrementarCantidad = (guitarra) => {
  guitarra.cantidad--
}

const incrementarCantidad = (guitarra) => {
  guitarra.cantidad++
}

const eliminarProducto = (id) => {
  carrito.value = carrito.value.filter(producto => producto.id !== id)
}

const vaciarCarrito = () => {
  carrito.value = []
}

const guardarLocalStorage = () => {
  localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

onMounted(() => {
  guitarras.value = db
  guitarra.value = db[3]
  const carritoStorage = localStorage.getItem('carrito')
  if (carritoStorage) {
    carrito.value = JSON.parse(carritoStorage)
  }
})
</script>

<template>
  <Header
    :carrito="carrito"
    :guitarra="guitarra"
    @decrementar-cantidad="decrementarCantidad"
    @incrementar-cantidad="incrementarCantidad"
    @agregar-carrito="agregarCarrito"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <div class="row mt-5">
      <Guitarra
          v-for="guitarra in guitarras"
          :guitarra="guitarra"
          @agregar-carrito="agregarCarrito(guitarra)"
      />
    </div>
  </main>
  <Footer />
</template>
