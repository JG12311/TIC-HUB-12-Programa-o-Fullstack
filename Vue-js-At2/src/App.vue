<script lang="ts">
import { defineComponent } from 'vue';
import type { Product } from './model/product.model';
import ProductCard from './components/productcard.vue';
import { Cart } from './model/cart.model';

export default defineComponent({
  components: { ProductCard },
  data() {
    return {
      cart: new Cart(),
      products: [
        {
          id: 1,
          name: 'Brinquedo',
          price: 15.0,
          category: { id: 1, title: 'Infantil' }
        },
        {
          id: 2,
          name: 'Camiseta',
          price: 32.50,
          category: { id: 2, title: 'Roupas' }
        }
      ] as Product[]
    };
  },
  methods: {
    addToCart(product: Product) {
      this.cart.addItem(product);
    },
    decreaseQuantity(productId: number) {
      this.cart.removeItem(productId);
    },
    removeFromCart(productId: number) {
      this.cart.deleteItem(productId);
    }
  }
});
</script>

<template>
  <main class="container">
    <div class="products-list">
      <ProductCard
        v-for="product in products"
        :key="product.id"
        :product="product"
        @add-to-cart="addToCart"
      />
    </div>

    <section class="cart-section">
      <h2>Meu carrinho</h2>
      <ul>
        <li v-for="item in cart.items" :key="item.product.id" class="cart-item">
          <span class="item-name">{{ item.product.name }}</span>
          <span class="item-details">
            ({{ item.quantity }}x) - R$ {{ (item.product.price * item.quantity).toFixed(2) }}
          </span>
          <div class="item-actions">
            <button @click="decreaseQuantity(item.product.id)">-</button>
            <button @click="removeFromCart(item.product.id)">Excluir</button>
          </div>
        </li>
      </ul>
      <div v-if="cart.items.length === 0">
        <p>O carrinho está vazio.</p>
      </div>
      <div v-else class="cart-summary">
        <hr />
        <p>Total de itens: {{ cart.totalItems }}</p>
        <p>Preço Final: R$ {{ cart.totalPrice.toFixed(2) }}</p>
      </div>
    </section>
  </main>
</template>