<script setup>
// import HelloWorld from './components/HelloWorld.vue'
import { ref, computed } from 'vue'

const productos = ref([
  { id: 1, nombre: 'Arroz 1 kg', precio: 4500, stock: 10, emoji: '🍚' },
  { id: 2, nombre: 'Leche 1 L', precio: 3800, stock: 8, emoji: '🥛' },
  { id: 3, nombre: 'Queso 250 g', precio: 9200, stock: 3, emoji: '🧀' },
  { id: 4, nombre: 'Huevos x12', precio: 14000, stock: 2, emoji: '🥚' },
  { id: 5, nombre: 'Banano', precio: 800, stock: 0, emoji: '🍌' },
  { id: 6, nombre: 'Croissant', precio: 3000, stock: 7, emoji: '🥐' }
]) 

const carrito = ref([])

</script>

<template>
  <h1 class="titulo">🏪 Tienda Don Pepe</h1>
  <h2>Catálogo</h2>
  
  <section class="tarjeta">
    <div
    v-for="p in productos"
    :key="p.id"
    class="contenedor"
    :class="{ Agotado: p.stock === 0 }">
    <p>{{ p.emoji }}</p>
    <p>{{ p.nombre }}</p>
    <p>${{ p.precio }}</p>
    
    <p v-if="p.stock === 0" class="texto-agotado">Agotado</p>
    <p v-else>Stock: {{ p.stock }}</p>

    <button @click="agregarAlCarrito(p.id)" :disabled="p.stock === 0" class="boton_agregar">Agregar</button>
  </div>
  
  <div class="contendor1">
    <h2>Mi carrito</h2>
  </div>
  </section>

</template>

<style scoped>
.titulo{
  background-color: cyan;
  margin: 0 auto;
}

.tarjeta{
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}

.contenedor{
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  width: 200px;
  height: 210px;
  margin: 0 auto; /* Centra la tarjeta horizontalmente */
  text-align: center;
}

.contenedor.Agotado {
  background: #e0e0e0;
  color: #888;
  opacity: 0.7;
}

.contenedor.Agotado .texto-agotado {
  color: red;
  font-weight: bold;
}

.boton_agregar{
  background-color: cyan;
  border-radius: 0.3rem;
}

</style>