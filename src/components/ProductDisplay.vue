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
        return { backgroundColor: 'var(--gray)', backgroundImage: 'none !important' }
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
    },

    warnarating(category) {
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
  <!-- Loading Indicator -->
  <div v-if="loading">
    <SkeletonLoader />
  </div>

  <!-- kalau konten sudah siap -->
  <div v-else class="latar" :style="gantibg(products.data.category)">
      <div class="container">

        <!-- kalau produk men atau women -->
        <div class="catalog-card" v-if="ProductAvailable === true">
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
                  <!-- <div v-for="i in 5" :key="i" class="circle" :class="{filled : i <= Math.round(products.data.rating.rate) }"></div> -->
                  <div v-for="i in 5" :key="i" class="circle"
                    :class="{ filled: i <= Math.round(products.data.rating.rate) }"
                    :style="warnarating(products.data.category)"></div>
                  <div v-for="i in 5" :key="i" class="circle"
                    :class="{ empty: i <= 5 - Math.round(products.data.rating.rate) }"
                    :style="warnarating(products.data.category)"></div>
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
        </div>

        <!-- kalau produk selain men dan women -->
        <div class="catalog-card" v-else>
          <div class="row-unavailable">
            <div style="z-index: 1;" class="sad-bg">
              <svg width="1028" height="439" viewBox="0 0 1028 439" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path
                  d="M221.295 114.085C221.295 99.5795 226.49 87.1321 236.879 76.7429C247.268 66.3537 259.716 61.1591 274.222 61.1591C288.727 61.1591 301.175 66.3537 311.564 76.7429C321.953 87.1321 327.148 99.5795 327.148 114.085C327.148 128.591 321.953 141.038 311.564 151.428C301.175 161.817 288.727 167.011 274.222 167.011C264.616 167.011 255.795 164.561 247.759 159.661C239.722 154.956 233.253 148.585 228.352 140.548C223.648 132.707 221.295 123.886 221.295 114.085ZM556.494 114.085C556.494 99.5795 561.689 87.1321 572.078 76.7429C582.467 66.3537 594.915 61.1591 609.42 61.1591C623.926 61.1591 636.374 66.3537 646.763 76.7429C657.152 87.1321 662.347 99.5795 662.347 114.085C662.347 128.591 657.152 141.038 646.763 151.428C636.374 161.817 623.926 167.011 609.42 167.011C599.815 167.011 590.994 164.561 582.957 159.661C574.92 154.956 568.452 148.585 563.551 140.548C558.847 132.707 556.494 123.886 556.494 114.085Z"
                  fill="black" fill-opacity="0.1" />
                <path
                  d="M454.045 227.21C528.142 227.21 596.26 236.815 658.399 256.026C720.734 275.432 778.071 303.071 830.409 338.943V400.102C811.003 385.989 787.088 372.757 758.665 360.408C730.438 348.254 699.368 337.571 665.456 328.358C631.74 319.145 596.848 311.892 560.78 306.599C524.712 301.503 489.134 298.955 454.045 298.955C407.392 298.955 360.053 303.463 312.027 312.48C264.001 321.497 219.406 333.651 178.241 348.94C137.077 364.23 103.557 381.284 77.6818 400.102V338.943C130.02 303.071 187.259 275.432 249.398 256.026C311.733 236.815 379.949 227.21 454.045 227.21Z"
                  fill="black" fill-opacity="0.1" />
                <path d="M133 38.5C235.4 53.7 299.333 28.5 318.5 14" stroke="black" stroke-opacity="0.1"
                  stroke-width="25" />
                <path d="M739 38.3236C636.324 53.4142 572.218 28.3956 553 14" stroke="black" stroke-opacity="0.1"
                  stroke-width="25" />
              </svg>
            </div>
            <div style="z-index: 2;">
              <h2 class="unavailable-message">This product is unavailable to show</h2>
              <button class="btn-unavailable" @click="nextProduct()">Next Product</button>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>




<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
@import url("../assets/stylesheet.css");
</style>
