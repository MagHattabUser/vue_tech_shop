<template>
    <Header />
    <div class="container mx-auto py-8">
        <div v-if="product">
            <div class="flex flex-col lg:flex-row items-center">
                <!-- Изображение товара -->
                <div class="flex-1 mb-8 lg:mb-0 lg:w-1/2">
                    <img :src="product.image" :alt="product.name"
                        class="w-full h-auto object-cover rounded-lg shadow-md" />
                </div>

                <!-- Информация о товаре -->
                <div class="flex-1 lg:ml-8">
                    <h1 class="text-3xl font-bold text-gray-800 mb-4">{{ product.name }}</h1>
                    <p class="text-gray-600 mb-6">{{ product.description }}</p>

                    <!-- Блок с характеристиками -->
                    <div class="bg-gray-100 p-4 rounded-lg shadow-md mb-6">
                        <h2 class="text-lg font-semibold text-gray-700 mb-2">Характеристики:</h2>
                        <ul class="list-disc list-inside text-gray-600">
                            <li v-for="(spec, index) in product.specs" :key="index">
                                {{ spec }}
                            </li>
                        </ul>
                    </div>

                    <!-- Цена товара -->
                    <div class="text-xl font-semibold text-blue-800 mb-6">
                        {{ formattedPrice(product.price) }} ₽
                    </div>

                    <!-- Кнопка "Добавить в корзину" -->
                    <button @click="addToCart(product)"
                        class="px-6 py-3 bg-blue-600 text-white font-semibold rounded-lg hover:bg-blue-700 transition duration-300">
                        Добавить в корзину
                    </button>
                </div>
            </div>
        </div>
        <div v-else>
            <p>Товар не найден.</p>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Header from "@/components/Header.vue";
import { useCartStore } from "@/store/cartStore";

// Определение интерфейса для структуры данных товара
interface Product {
    id: number;
    name: string;
    description: string;
    price: number;
    image: string;
    specs: string[];
}

export default defineComponent({
    name: "Product",
    components: { Header },
    data() {
        return {
            product: null as Product | null, // Товар может быть либо объектом Product, либо null
        };
    },
    mounted() {
        this.fetchProduct();
    },
    methods: {
        fetchProduct(): void {
            const products: Product[] = [
                {
                    id: 1,
                    name: "Смартфон",
                    description: "Мощный смартфон с отличной камерой.",
                    price: 29999,
                    image: "https://via.placeholder.com/150",
                    specs: [
                        "Камера: 12 МП",
                        "Процессор: 8-ядерный",
                        "Экран: 6.5 дюйма AMOLED",
                        "Аккумулятор: 4000 мАч"
                    ]
                },
                {
                    id: 2,
                    name: "Ноутбук",
                    description: "Легкий и производительный ноутбук.",
                    price: 69999,
                    image: "https://via.placeholder.com/150",
                    specs: [
                        "Процессор: Intel Core i7",
                        "ОЗУ: 16 ГБ",
                        "Хранилище: 512 ГБ SSD",
                        "Диагональ экрана: 15.6 дюйма"
                    ]
                },
                {
                    id: 3,
                    name: "Наушники",
                    description: "Беспроводные наушники с шумоподавлением.",
                    price: 8999,
                    image: "https://via.placeholder.com/150",
                    specs: [
                        "Шумоподавление: Активное",
                        "Автономность: 20 часов",
                        "Тип подключения: Bluetooth 5.0",
                        "Защита: IPX4"
                    ]
                },
                {
                    id: 4,
                    name: "Кофемашина",
                    description: "Эспрессо и капучино дома каждый день.",
                    price: 19999,
                    image: "https://via.placeholder.com/150",
                    specs: [
                        "Тип: Автоматическая",
                        "Мощность: 1450 Вт",
                        "Объем резервуара для воды: 1.8 л",
                        "Капучинатор: Есть"
                    ]
                },
                {
                    id: 5,
                    name: "Умные часы",
                    description: "Следите за здоровьем и оставайтесь на связи.",
                    price: 12999,
                    image: "https://via.placeholder.com/150",
                    specs: [
                        "Мониторинг: Шаги, сон, пульс",
                        "Экран: AMOLED, 1.4 дюйма",
                        "Автономность: 7 дней",
                        "Водонепроницаемость: IP68"
                    ]
                }
            ];

            const paramId = Array.isArray(this.$route.params.id)
                ? this.$route.params.id[0]
                : this.$route.params.id;

            // Преобразуем параметр id в число
            const productId = parseInt(paramId, 10);

            if (isNaN(productId)) {
                console.error("Некорректный ID товара!");
                return;
            }

            this.product = products.find((prod) => prod.id === productId) || null;

            if (!this.product) {
                console.error("Товар с таким id не найден!");
            }
        },

        formattedPrice(price: number): string {
            return price.toLocaleString("ru-RU");
        },
        addToCart(product: Product): void {
            const cartStore = useCartStore();
            cartStore.addToCart(product);
        },
    },
});
</script>
