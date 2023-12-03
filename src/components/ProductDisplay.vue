<script>
import SkeletonLoader from "./SkeletonLoader.vue";

export default {
  name: 'ProductDisplay',
  components: {
    SkeletonLoader,
  },

  // data
  data() {
    return {
      index: 0,
      products: {},
      loading: false,
      ProductAvailable: false,
    }
  },

  // get api data
  methods: {
    async fetchProducts() {
      try {
        // Fetch data from the API
        const url = `https://fakestoreapi.com/products/${this.index}`;
        const response = await fetch(url);
        const data = await response.json();
        console.log(data);
        return data;

      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },

    async nextProduct() {
      this.loading = true;

      // Increment index or reset to 1
      if (this.index !== 20) {
        this.index++;
      } else {
        this.index = 1;
      }

      // Fetch data from API
      const data = await this.fetchProducts();

      // Check if the product category is valid
      if (data.category === "men's clothing" || data.category === "women's clothing") {
        this.products = { data };
        this.ProductAvailable = true;
      } else {
        this.products = { data };
        this.ProductAvailable = false;
      }

      this.loading = false;
    },

    gantibg(category) {
      console.log(category)
      if (category === "men's clothing") {
        return { backgroundColor: 'var(--lightBlue)' }
      } else if (category === "women's clothing") {
        return { backgroundColor: 'var(--lightMagenta)' }
      } else if (category !== "men's clothing" || category === "women's clothing") {
        return { backgroundColor: 'var(--gray)' }
      }
    },

    gantiwarna(category) {
      if (category === "men's clothing") {
        return { backgroundColor: 'var(--navyBlue)', color: 'var(--navyBlue)', borderColor: 'var(--navyBlue)' }
      } else if (category === "women's clothing") {
        return { backgroundColor: 'var(--magentaPurple)', color: 'var(--magentaPurple)', borderColor: 'var(--magentaPurple)' }
      } else {
        return { backgroundColor: 'var(--black)', color: 'var(--black)', borderColor: 'var(--black)' }
      }
    }
  },

  mounted() {
    this.nextProduct();
  },


}


</script>

<template>
  <div class="latar" :style="gantibg(products.data.category)">
    <div v-if="loading">
      <SkeletonLoader />
    </div>
    <div class="container">
      <!-- Loading Indicator -->
      <div class="catalog-card" v-if="ProductAvailable === true">

        <!-- <div v-if="ProductAvailable === true"> -->

        <!-- konten kiri -->
        <div class="catalog-left-col">
          <img :src="products.data.image" class="product-img" alt="gambar produk">
        </div>

        <!-- konten kanan -->
        <div class="catalog-right-col">
          <h1 class="product-name" :style="gantiwarna(products.data.category)">{{ products.data.title }}</h1>
          <div class="product-info">
            <p>{{ products.data.category }}</p>
            <div class="product-rate">
              <p class="product-rating">
                {{ products.data.rating.rate }}/5
              </p>
              <div class="product-rating-circle">
                <div v-for="i in 5" :key="i" class="circle" :class="{filled : i <= Math.round(products.data.rating.rate) }"></div>
              </div>
            </div>
          </div>
          <div>
            <hr>
            <p class="product-desc">{{ products.data.description }}</p>
          </div>

          <div class="bottom-row">
            <hr>
            <p class="product-price" :style="gantiwarna(products.data.category)">
              ${{ products.data.price }}
            </p>
            <button class="btn-buy" :style="gantiwarna(products.data.category)">Buy now</button>
            <button @click="nextProduct()" class="btn-next" :style="gantiwarna(products.data.category)">Next
              product</button>
          </div>
        </div>

        <!-- </div> -->


      </div>
      <div class="catalog-card" v-else>
        <div class="row-unavailable">
          <h2 class="unavailable-message">This product is unavailable to show</h2>
          <button class="btn-unavailable" @click="nextProduct()">Next Product</button>
        </div>
      </div>
    </div>
  </div>
</template>




<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
@import url("../assets/stylesheet.css");
</style>
