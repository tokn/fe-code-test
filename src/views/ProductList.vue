<template>
  <div class="wrapper">
    <label for="selector">
      Filter:
      <select v-model="selectedFilter" id="selector">
        <option value="All" selected>All</option>
        <option value="Purchased" selected>Purchased</option>
        <option value="Unpurchased" selected>Unpurchased</option>
        <option value="One time purchases" selected>One time purchases</option>
        <option value="Subscriptions" selected>Subscriptions</option>
      </select>
    </label>
    <h1>SELECTED FILTER: {{ selectedFilter }}</h1>

    <div class="products">
      <Product v-for="(product,idx) in products" :key="idx" :product="product" />
    </div>
  </div>
</template>

<script>
const productItems = require("@/assets/products.json");
import Product from "../components/Product.vue";

export default {
  name: "ProductList",
  components: {
    Product
  },
  computed: {
    products() {
      productItems.sort((a,b) => (a.order > b.order) ? 1 : -1);

      if (this.selectedFilter !== "All") {
        let selectedFilter = this.filters[this.selectedFilter];
        let products = [...productItems];
        let filterProperty = selectedFilter["property"];
        let filterValue = selectedFilter["value"];
        let filteredProducts = products.filter(prod => prod[filterProperty] === filterValue);

        return filteredProducts;
      } else {
        return productItems;
      }
    }
  },
  data() {
    return {
      select: "All",
      selectedFilter: "All",
      allProducts: productItems,
      filters: {
        Purchased: {
          property: "purchased",
          value: true
        },
        Unpurchased: {
          property: "purchased",
          value: false
        },
        "One time purchases": {
          property: "type",
          value: "onetime"
        },
        Subscriptions: {
          property: "type",
          value: "recurring"
        }
      }
    };
  },
  watch: {
    select: function(oldVal, newVal) {
      this.selectedFilter = newVal;
    }
  }
};
</script>
