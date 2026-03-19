<template>
  <main class="min-h-[calc(100vh-6rem)]">
    <div class="flex flex-col gap-10">
    
      <section>
        <h2 class="text-xl font-semibold mb-4">Produtos</h2>
        <div class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-6">
          <ProductCard
            v-for="product in products"
            :key="product.id"
            :product="product"
            @add-to-cart="addToCart"
          />
        </div>
      </section>

    
      <aside class="bg-white rounded-2xl p-6 shadow-lg">
        <h2 class="text-xl font-semibold mb-4">Carrinho</h2>

        <div v-if="cartItems.length === 0" class="text-sm text-slate-500">
          Seu carrinho está vazio. Adicione um produto para ver o resumo aqui.
        </div>

        <DataView v-else :value="cartItems" class="space-y-4">
          <template #list="slotProps">
            <div class="flex flex-col gap-4">
              <div v-for="(item, index) in slotProps.items" :key="index" class="flex flex-col gap-3 p-4 border rounded-xl bg-slate-50">
              <div class="flex items-start justify-between gap-3">
                <div class="flex items-center gap-3">
                  <img
                    v-if="item.product.image"
                    :src="item.product.image"
                    :alt="item.product.name"
                    class="w-14 h-14 rounded-lg object-cover"
                  />
                  <div>
                    <div class="font-semibold">{{ item.product.name }}</div>
                    <div class="text-xs text-slate-500">
                      R$ {{ item.product.price.toFixed(2) }}
                    </div>
                  </div>
                </div>

                <div class="text-right">
                  <div class="text-xs text-slate-500">Subtotal</div>
                  <div class="font-semibold">
                    R$ {{ (item.product.price * item.quantity).toFixed(2) }}
                  </div>
                </div>
              </div>

              <div class="flex items-center justify-between gap-3">
                <div class="flex items-center gap-2">
                  <span class="text-sm text-slate-700">Quantidade:</span>
                  <InputNumber
                    v-model="item.quantity"
                    :min="1"
                    :showButtons="true"
                    buttonLayout="horizontal"
                    style="width: 9rem"
                  />
                </div>

                <Button
                  icon="pi pi-trash"
                  class="p-button-danger p-button-text"
                  @click="removeFromCart(item.product.id)"
                />
              </div>
              </div>
            </div>
          </template>
        </DataView>

        <div v-if="cartItems.length" class="mt-6 border-t pt-4">
          <div class="flex items-center justify-between text-sm text-slate-600">
            <span>Total</span>
            <span class="font-semibold">R$ {{ cartTotal.toFixed(2) }}</span>
          </div>

          <Button
            label="Finalizar compra"
            icon="pi pi-check"
            class="w-full mt-4"
            :disabled="cartItems.length === 0"
          />
        </div>
      </aside>
    </div>
  </main>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue'
import ProductCard from '@/components/ProductCard.vue'
import type { Product } from '@/models/product.model'
import type { CartItem } from '@/models/cart-item.model'

export default defineComponent({
  name: 'HomeView',
  components: { ProductCard },
  data() {
    return {
      products: [
        {
          id: 'p1',
          name: 'Tênis Esportivo',
          description: 'Leve e confortável para todas as atividades.',
          price: 249.9,
          image: 'https://picsum.photos/seed/tenis/600/400',
        },
        {
          id: 'p2',
          name: 'Fone de Ouvido',
          description: 'Som cristalino com cancelamento de ruído ativo.',
          price: 699.0,
          image: 'https://picsum.photos/seed/fone/600/400',
        },
        {
          id: 'p3',
          name: 'Mochila Compacta',
          description: 'Organização inteligente para o dia a dia.',
          price: 159.9,
          image: 'https://picsum.photos/seed/mochila/600/400',
        },
        {
          id: 'p4',
          name: 'Relógio Smart',
          description: 'Monitoramento de saúde e notificações direto no pulso.',
          price: 899.0,
          image: 'https://picsum.photos/seed/relogio/600/400',
        },
      ] as Product[],
      cartItems: [] as CartItem[],
    }
  },
  computed: {
    cartTotal(): number {
      return this.cartItems.reduce(
        (total, item) => total + item.product.price * item.quantity,
        0,
      )
    },
  },
  methods: {
    addToCart(product: Product) {
      const existing = this.cartItems.find((item) => item.product.id === product.id)

      if (existing) {
        existing.quantity += 1
      } else {
        this.cartItems.push({ product, quantity: 1 })
      }
    },
    removeFromCart(productId: string) {
      this.cartItems = this.cartItems.filter((item) => item.product.id !== productId)
    },
  },
})
</script>
