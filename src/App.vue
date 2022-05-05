<template>
  <div class="container py-4">
    <div class="flex space-x-2">
      <div class="w-3/4">
        <h2 class="text-2xl text-white">Produits</h2>
        <hr />
        <div class="flex flex-wrap -mx-1">
          <div
            v-for="(product, productIndex) in Products"
            :key="productIndex"
            class="w-1/5 px-1"
          >
            <div class="card bg-violet-500 text-white">
              <h4 class="product-name">{{ product.name }}</h4>
              <hr class="border-gray-200 my-2" />
              <div class="flex justify-between">
                <span class="badge bg-blue-500">{{ product.price }}€</span>
                <button class="badge bg-teal-500" @click="addToCart(product)">
                  <i class="bi bi-cart-plus"></i>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="w-1/4">
        <h2 class="text-2xl text-white">Panier</h2>
        <hr />
        <div class="card bg-blue-500 text-white text-lg">
          <div class="flex justify-between text-xl bg-blue-600 rounded px-2 font-semibold">
            <span class=""
              >{{ this.panier.length }} produit{{
                this.panier.length > 1 ? "s" : ""
              }}
            </span>
            <span>{{ this.getTotal }} €</span>
          </div>
          <div class="" v-if="this.panier.length > 0">
            <div
              v-for="(panierItem, panierItemIndex) in panier"
              :key="panierItemIndex"
              class="flex justify-between"
            >
              <div>
                <span>{{ panierItem.name }}</span>
              </div>
              <div>
                <span class="font-semibold text-gray-200"
                  >{{ panierItem.price }} € |
                </span>
                <button
                  class="text-red-300 text-sm badge bg-red-400"
                  @click="deleteFromCart(panierItem)"
                >
                  <i class="bi bi-trash-fill"></i>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Products from "./services/products";
export default {
  name: "APP",
  data() {
    return {
      Products,
      loading: false,
      panier: [],
    };
  },
  computed: {
    getTotal() {
      let sum = 0;
      if (this.panier.length > 0) {
        sum = this.panier.reduce((total, value) => {
          return total + value.price;
        }, 0);
      }
      return sum;
    },
  },
  methods: {
    loadProducts() {},
    addToCart(product) {
      if (!this.panier.includes(product)) {
        this.panier.push(product);
      }
    },
    deleteFromCart(product) {
      let productIndex = this.panier.indexOf(product);
      let temp = this.panier.splice(productIndex, 1);
    },
  },
  mounted() {
    this.loadProducts();
  },
};
</script>
<style lang="postcss">
.badge {
  @apply text-white rounded font-semibold px-2;
}
.card {
  @apply rounded shadow p-2 my-1;
}
.product-name {
  @apply text-center text-xl font-semibold;
}
</style>