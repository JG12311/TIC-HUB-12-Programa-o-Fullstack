<template>
  <article class="product-card">
    <h2>{{ product.name }}</h2>
    <p class="category">{{ product.category.title }}</p>
    <p class="price">Preço: R$ {{ product.price.toFixed(2) }}</p>
    <button @click="addToCart">Adicionar</button>
  </article>
</template>

<script lang="ts">
import { defineComponent, type PropType } from 'vue';
import type { Product } from '../model/product.model';

export default defineComponent({
  name: 'ProductCard',
  props: {
    product: {
      type: Object as PropType<Product>,
      required: true
    }
  },
  emits: ['add-to-cart'],
  methods: {
    addToCart() {
      this.$emit('add-to-cart', this.product);
    }
  }
});
</script>
