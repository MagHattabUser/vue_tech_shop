<template>
    <div class="container mx-auto p-4">
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
        <div v-for="product in products" :key="product.id"
          class="bg-white shadow rounded-lg p-4 flex flex-col items-center transform transition-all duration-300 hover:scale-105 hover:shadow-xl">
          <img :src="product.image" :alt="product.name" class="w-32 h-32 object-cover mb-4 rounded-md"
            @click="goToProduct(product)" />
          <h2 class="text-lg font-semibold mb-2">{{ product.name }}</h2>
          <p class="text-gray-500 text-sm mb-2">{{ product.description }}</p>
          <span class="text-blue-800 font-bold text-lg">
            {{ formatPrice(product.price) }} ₽
          </span>
          <!-- Контейнер для кнопок, используем flex для выравнивания в ряд -->
          <div class="flex space-x-2 mt-4">
            <button @click="goToProduct(product)"
              class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600 transition w-full">
              Подробнее
            </button>
            <button @click="addToCart(product)"
              class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600 transition w-full">
              В корзину
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  import { useCartStore } from '@/store/cartStore';
  import смартфон from '@/assets/смартфон.jpg'
  import ноутбук from '@/assets/ноутбук.jpg'
  import наушники from '@/assets/наушники.jpg'
  import кофемашина from '@/assets/кофемашина.jpg'
  import часы from '@/assets/часы.jpg'
  
  export default defineComponent({
    name: 'ProductCatalog',
    data() {
      return {
        products: [
          {
            id: 1,
            name: 'Смартфон',
            description: 'Мощный смартфон с отличной камерой.',
            price: 29999,
            image: смартфон,
          },
          {
            id: 2,
            name: 'Ноутбук',
            description: 'Легкий и производительный ноутбук.',
            price: 69999,
            image: ноутбук,
          },
          {
            id: 3,
            name: 'Наушники',
            description: 'Беспроводные наушники с шумоподавлением.',
            price: 8999,
            image: наушники,
          },
          {
            id: 4,
            name: 'Кофемашина',
            description: 'Эспрессо и капучино дома каждый день.',
            price: 19999,
            image: кофемашина,
          },
          {
            id: 5,
            name: 'Умные часы',
            description: 'Следите за здоровьем и оставайтесь на связи.',
            price: 12999,
            image: часы,
          },
        ],
      };
    },
    methods: {
      addToCart(product: { id: number, name: string, description: string, price: number, image: string }) {
        const cartStore = useCartStore();
        console.log(product)
        cartStore.addToCart(product);
      },
      formatPrice(price: number): string {
        return price.toLocaleString('ru-RU');
      },
      goToProduct(product: { id: number }) {
        this.$router.push(`/product/${product.id}`);
      },
    },
  });
  </script>
  