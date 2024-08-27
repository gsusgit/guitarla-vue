<script setup>
import {ref, onMounted} from 'vue'
import {db} from './data/guitarras'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import Guitarra from './components/Guitarra.vue'

const guitarras = ref([])
const carrito = ref([])

const agregarCarrito = (guitarra) => {
  guitarra.cantidad = 1
  carrito.value.push(guitarra)
}

onMounted(() => {
  guitarras.value = db
})
</script>

<template>
  <Header />
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
