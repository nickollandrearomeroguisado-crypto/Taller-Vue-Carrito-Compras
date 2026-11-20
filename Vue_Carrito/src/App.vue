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

function agregarAlCarrito(producto) {
  if (producto.stock === 0) return

  const enCarrito = carrito.value.find(item => item.id === producto.id)

  if (enCarrito) {
    enCarrito.cantidad++
  } else {
    carrito.value.push({ ...producto, cantidad: 1 })
  }

  producto.stock--
}

// Función para eliminar tarea
const eliminarTarea = (productoId) => {

  const itemEnCarrito = carrito.value.find(item => item.id === productoId);
  
  if (itemEnCarrito) {
    // 2. Buscar el producto original en el inventario
    const productoOriginal = productos.value.find(prod => prod.id === productoId);
    
    // 3. Devolver las unidades al stock
    if (productoOriginal) {
      productoOriginal.stock += itemEnCarrito.cantidad;
    }
    // carrito.value.splice(productoId, 1)

    // 4. Eliminar el producto del carrito
    carrito.value = carrito.value.filter(item => item.id !== productoId);
  }
}

// Calcula el total sumando las cantidades de cada producto
const totalProductos = computed(() => {
  return carrito.value.reduce((total, item) => total + item.cantidad, 0);
});

//total
const total = computed(() => {
  return carrito.value.reduce((acumulador, item) => {
    return acumulador + (item.precio * item.cantidad);
  }, 0);
});

//descuento
const tieneDescuento = computed(() => total.value > 50000);

const descuento = computed(() => {
  return tieneDescuento.value ? total.value * 0.10 : 0;
})

const totalFinal = computed(() => {
  return total.value - descuento.value;
});

</script>

<template>
  <div class="titulo-tienda">
    <h1 class="titulo">🏪 Tienda Don Pepe</h1>
    <span class="carrito-c">{{ totalProductos }} 🛒</span>
  </div>

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
    
    <!--cuando el producto esta agotado-->
    <p v-if="p.stock === 0" class="texto-agotado">Agotado</p>
    <p v-else>Stock: {{ p.stock }}</p>

    <button @click="agregarAlCarrito(p)" :disabled="p.stock === 0" class="boton_agregar">Agregar 🛒</button>
  </div>

  <div class="contendor1">
    <h2>Mi carrito</h2>
    <p v-if="carrito.length === 0">Tu carrito está vacío</p>

    <p v-for="item in carrito" :key="item.id">
      {{ item.emoji }} {{ item.nombre }} x {{ item.cantidad }} {{ item.precio * item.cantidad }}

      <button class="btn-eliminar" @click="eliminarTarea(item.id)">X</button>
    </p>
    
    <p>Subtotal: ${{ total }}</p>

    <p>Descuento: ${{ descuento }}</p>

    <p>Total a pagar: ${{ totalFinal }}</p>

  </div>
  
  </section>

</template>

<style scoped>
/**titulo y carrito de compras*/
.titulo-tienda{
  background-color: cyan; /* Color de fondo */
  color: white;
  width: 100%;
  padding: 20px; /* Espacio interno para que el texto respire */
  box-sizing: border-box; /* Evita que el padding genere scroll horizontal */
  display: flex;
 justify-content: space-between;
  align-items: center; /* Opcional: los centra verticalmente */
  width: 100%;
}

.titulo{
  margin: 0;
}

.carrito-c{
  
}

/**..... */

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

<style>
body, html {
  margin: 0;
  padding: 0;
}
</style>