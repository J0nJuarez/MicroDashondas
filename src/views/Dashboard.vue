<template>
  <div class="p-6">
    <div v-if="loading" class="text-center mt-8">Cargando productos...</div>

    <div v-else>
      <div
        v-for="(productos, fecha) in data"
        :key="fecha"
        class="mb-12"
      >
        <h2 class="text-2xl font-bold mb-6">{{ fecha }}</h2>
        <div class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-5 gap-6">
          <div
            v-for="(item, i) in productos"
            :key="i"
            class="bg-white rounded-2xl shadow-md overflow-hidden hover:shadow-lg transition"
          >
            <a :href="item.url" target="_blank" rel="noopener noreferrer">
              <img
                :src="item.imagen"
                :alt="item.producto"
                class="w-full h-40 object-cover"
              />
              <div class="p-4">
                <h3 class="text-sm font-semibold line-clamp-2 mb-1">
                  {{ item.producto }}
                </h3>
                <p class="text-xs text-gray-500">{{ item.categoria }}</p>
                <p class="text-lg font-bold text-green-600 mt-2">
                  {{ item.precio ? `${item.precio} €` : "N/D" }}
                </p>
                <p
                  v-if="item.precioAnterior"
                  class="text-xs text-red-500 line-through"
                >
                  {{ item.precioAnterior }}
                </p>
              </div>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const data = ref({});
const loading = ref(true);

const fetchData = async () => {
  try {
    const res = await fetch(
      "https://script.googleusercontent.com/macros/echo?user_content_key=AehSKLgk4AsPoPNpZrQOuIGzAgZk8ioT1p_ZM6wfHHJkwbisjtTuaBJItukW7ypYsV56fGQQtfcdgCR91HucHrWXNd9czNdkocO6X9mPgrf3Op-LdG9JVrTNTXLhrVmeK_H38edmvIOaLfCiQx6bONXHet4geoT05xi7NcoSkGoCb107QTDOL9itqNpJJnwmZ0xdEWXuBezy16fRuAMBBgoqxa9hqQSeUNwY2Qdp7Uxas7XTHjtTd_E4WzD2KwRi-cmc7VUX9YEhyweP91N5Qj2vpvkfONGfSWfHo_ElZOeg&lib=MCfpaExoUtjkyJvdkDLwY4TZk4uhUiKxr"
    );
    const json = await res.json();
    data.value = json;
  } catch (err) {
    console.error("Error al cargar datos:", err);
  } finally {
    loading.value = false;
  }
};



onMounted(fetchData);
</script>

<style>
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>
