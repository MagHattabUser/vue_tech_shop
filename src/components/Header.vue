<template>
  <header class="bg-blue-800 text-white py-4 px-6">
    <div class="container mx-auto flex items-center justify-between">
      <a href="/" class="text-2xl font-bold hover:text-gray-300 transition">Логотип</a>
      <nav class="flex space-x-4">
        <a href="/" class="hover:text-gray-300 transition">Главная</a>
      </nav>
      <div
        class="relative"
        @mouseenter="showCart = true"
        @mouseleave="showCart = false"
      >
        <a href="/cart" class="flex items-center hover:text-gray-300 transition">
          <svg
            fill="#ffffff"
            version="1.1"
            id="Capa_1"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            viewBox="0 0 902.86 902.86"
            xml:space="preserve"
            stroke="#ffffff"
            class="w-6 h-6 mr-2"
          >
            <g>
              <path d="M671.504,577.829l110.485-432.609H902.86v-68H729.174L703.128,179.2L0,178.697l74.753,399.129h596.751V577.829z M685.766,247.188l-67.077,262.64H131.199L81.928,246.756L685.766,247.188z" />
              <path d="M578.418,825.641c59.961,0,108.743-48.783,108.743-108.744s-48.782-108.742-108.743-108.742H168.717 c-59.961,0-108.744,48.781-108.744,108.742s48.782,108.744,108.744,108.744c59.962,0,108.743-48.783,108.743-108.744 c0-14.4-2.821-28.152-7.927-40.742h208.069c-5.107,12.59-7.928,26.342-7.928,40.742 C469.675,776.858,518.457,825.641,578.418,825.641z M209.46,716.897c0,22.467-18.277,40.744-40.743,40.744 c-22.466,0-40.744-18.277-40.744-40.744c0-22.465,18.277-40.742,40.744-40.742C191.183,676.155,209.46,694.432,209.46,716.897z M619.162,716.897c0,22.467-18.277,40.744-40.743,40.744s-40.743-18.277-40.743-40.744c0-22.465,18.277-40.742,40.743-40.742 S619.162,694.432,619.162,716.897z" />
            </g>
          </svg>
          Корзина
        </a>
        <div
          v-if="showCart"
          class="absolute right-0 mt-2 w-64 bg-white text-black shadow-lg rounded-lg dropdown-menu"
        >
          <ul v-if="cart.length > 0" class="divide-y divide-gray-300">
            <li v-for="(item, index) in cart" :key="index" class="flex items-center p-4">
              <img :src="item.image" alt="Product Image" class="w-12 h-12 object-cover rounded-md mr-4" />
              <div>
                <h2 class="text-sm font-semibold">{{ item.name }}</h2>
                <p class="text-xs text-gray-500">Цена: {{ formattedPrice(item.price) }} ₽</p>
                <p class="text-xs text-gray-500">Количество: {{ item.quantity }}</p>
              </div>
            </li>
          </ul>
          <div v-else class="p-4 text-gray-500 text-sm">Корзина пуста.</div>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
import { useCartStore } from "@/store/cartStore";
import { computed } from "vue";

export default {
  name: "Header",
  data() {
    return {
      showCart: false,
    };
  },
  setup() {
    const cartStore = useCartStore();

    // Референсы на корзину и связанные данные из Pinia
    const cart = computed(() => cartStore.cart);
    const cartCount = computed(() => cartStore.cartCount);

    return {
      cart,
      cartCount,
      formattedPrice: (price) => new Intl.NumberFormat("ru-RU").format(price),
    };
  },
};
</script>

<style scoped>
.dropdown-menu {
  z-index: 50;
}
</style>
