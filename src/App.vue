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
            class="w-1/4 px-1"
          >
            <div class="card bg-violet-500 text-white">
              <div class="flex justify-between items-center">
                <h4 class="product-name">
                  {{ product.name }}
                </h4>
                <span class="badge text-sm bg-pink-600">{{
                  product.quantity
                }}</span>
              </div>
              <hr class="border-gray-200 my-2" />
              <div class="flex justify-between">
                <span class="badge bg-blue-500">{{ product.price }}€</span>
                <div class="flex space-x-1">
                  <button class="badge bg-teal-500" @click="dec(product)">
                    -
                  </button>
                  <span class="bg-white text-blue-900 font-bold px-4 rounded">{{
                    product.order ?? 0
                  }}</span>
                  <button class="badge bg-teal-500" @click="inc(product)">
                    +
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="w-1/4">
        <h2 class="text-2xl text-white">Panier</h2>
        <hr />
        <div class="card bg-blue-500 text-white text-lg">
          <div
            class="
              flex
              justify-between
              text-xl
              bg-blue-600
              rounded
              px-2
              font-semibold
            "
          >
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
                <span class="mr-1">{{ panierItem.name }} </span>
                <span class="badge bg-orange-500 text-sm"
                  >{{ panierItem.order }}
                </span>
              </div>
              <div>
                <span class="font-semibold text-gray-200"
                  >{{ panierItem.price * panierItem.order }} € |
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
import Products from "../src/services/Products";
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
          return total + value.price * value.order;
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
      product.order = 0;
      let productIndex = this.panier.indexOf(product);
      this.panier.splice(productIndex, 1);
    },
    inc(product) {
      if (!product.order) {
        product.order = 0;
      }
      if (product.order < product.quantity) {
        product.order++;
      }
      if (!this.panier.includes(product)) {
        this.panier.push(product);
      }
    },
    dec(product) {
      if (product.order > 1) {
        product.order--;
      } else {
        this.deleteFromCart(product);
      }
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