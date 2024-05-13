<template>
  <div class="containerCard">
    <img :src="product.image" alt="Product Image" class="productImage" />
    <div class="productDetail">
      <div class="topCard">
        <h2>{{ product.title }}</h2>
        <div>
          <div class="horizontal">
            <p class="category">Category: {{ product.category }}</p>
            <div class="ratingContainer">
              <p class="rating">{{ product.rating.rate }}/5</p>
              <div class="allStar">
                <div
                  v-for="index in 5"
                  :key="index"
                  class="star"
                  :class="{
                    'fullCircle-men':
                      index <= roundedRating &&
                      product.category === 'men\'s clothing',
                    'fullCircle-women':
                      index <= roundedRating &&
                      (product.category === 'women\'s clothing' ||
                        product.category === 'jewelery'),
                    'fullCircle-default':
                      index <= roundedRating &&
                      product.category !== 'men\'s clothing' &&
                      product.category !== 'women\'s clothing' &&
                      product.category !== 'jewelery',
                    'emptyCircle-men':
                      index > roundedRating &&
                      product.category === 'men\'s clothing',
                    'emptyCircle-women':
                      index > roundedRating &&
                      (product.category === 'women\'s clothing' ||
                        product.category === 'jewelery'),
                    'emptyCircle-default':
                      index > roundedRating &&
                      product.category !== 'men\'s clothing' &&
                      product.category !== 'women\'s clothing' &&
                      product.category !== 'jewelery',
                  }"
                ></div>
              </div>
            </div>
          </div>
          <hr />
        </div>
        <p class="description">Description: {{ product.description }}</p>
      </div>
      <div class="botCard">
        <hr />
        <p class="price">${{ product.price }}</p>
        <div class="horizontal buttonContainer">
          <button :style="buttonBuyStyle" @click="buyNow">Buy now</button>
          <button
            @click="nextProduct"
            class="nextButton"
            :style="buttonNextStyle"
          >
            Next product
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    product: Object,
  },
  computed: {
    buttonNextStyle() {
      if (!this.product) {
        return {}; // Return an empty object if no product is currently displayed
      }
      if (this.product.category === "men's clothing") {
        return {
          borderColor: "#002772",
          color: "#002772",
          backgroundColor: "white",
        };
      } else if (
        this.product.category === "women's clothing" ||
        this.product.category === "jewelery"
      ) {
        return {
          borderColor: "#720060",
          color: "#720060",
          backgroundColor: "white",
        }; // Pink color for women's clothing and jewelery
      } else {
        return {
          borderColor: "#1E1E1E",
          color: "#1E1E1E",
          backgroundColor: "white",
        };
      }
    },
    buttonBuyStyle() {
      if (!this.product) {
        return {}; // Return an empty object if no product is currently displayed
      }
      if (this.product.category === "men's clothing") {
        return {
          borderColor: "#002772",
          color: "white",
          backgroundColor: "#002772",
        };
      } else if (
        this.product.category === "women's clothing" ||
        this.product.category === "jewelery"
      ) {
        return {
          borderColor: "#720060",
          color: "white",
          backgroundColor: "#720060",
        }; // Pink color for women's clothing and jewelery
      } else {
        return {
          borderColor: "#1E1E1E",
          color: "white",
          backgroundColor: "#1E1E1E",
        };
      }
    },
    roundedRating() {
      return Math.round(this.product.rating.rate);
    },
  },
  methods: {
    buyNow() {
      // Trigger navigation to another route using Vue Router
      this.$router.push('/checkout'); // Replace 'Checkout' with the name of your checkout route
    },
    nextProduct() {
      this.$emit("next");
    },
  },
};
</script>

<style>
@import "../assets/style/ProductDetail.css";
</style>
