<template>
  <div>
    <div :style="productStyle" class="container">
      <ProductDetail
        :product="currentProduct"
        v-if="currentProduct"
        @next="nextProduct"
        class="productComponent"
      />
      <EmptyProduct
        v-if="!currentProduct && !loading"
        @fetch-next-product="fetchNextProduct"
        class="productComponent"
      />
      <Loader v-if="loading" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ProductDetail from "@/components/ProductDetail.vue";
import EmptyProduct from "@/components/EmptyProduct.vue"; // Import EmptyProduct component
import Loader from "@/components/LoaderSpinner.vue";

export default {
  components: {
    ProductDetail,
    EmptyProduct,
    Loader,
  },
  data() {
    return {
      products: [],
      currentIndex: 0,
      loading: false,
    };
  },
  computed: {
    currentProduct() {
      return this.products[this.currentIndex];
    },
    productStyle() {
      if (!this.currentProduct) {
        return { backgroundColor: "#D8D7D7",
        backgroundImage: "none"
         }; // Return an empty object if no product is currently displayed
      }
      if (this.currentProduct.category === "men's clothing") {
        return {
          backgroundColor: "#D6E6FF",
        };
      } else if (
        this.currentProduct.category === "women's clothing" ||
        this.currentProduct.category === "jewelery"
      ) {
        return {
          backgroundColor: "#FDE2FF",
        };
      } else if (this.currentProduct.category === "electronics") {
        return {
          backgroundColor: "#1E1E1E",
        };
      } else {
        return { backgroundColor: "#D8D7D7" };
      }
    },
  },
  created() {
    this.fetchProduct(1);
  },
  methods: {
    async fetchProduct(id) {
      this.loading = true;
      try {
        const response = await axios.get(
          `https://fakestoreapi.com/products/${id}`
        );
        if (response.data) {
          this.products.push(response.data);
        } else {
          this.loading = false; // Set loading to false if response data is empty
        }
      } catch (error) {
        console.error("Error fetching product:", error);
      } finally {
        this.loading = false; // Make sure to set loading to false in all cases
      }
    },
    fetchNextProduct() {
      this.fetchProduct(1); // Fetch product with id 1 (next product)
    },
    nextProduct() {
      this.currentIndex++;
      if (this.currentIndex >= this.products.length) {
        const nextProductId = this.products.length + 1;
        this.fetchProduct(nextProductId);
      }
    },
  },
};
</script>

<style scoped>
/* Add your styles here */
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 67vh;
  background-image: url('../assets/images/bg-pattern.svg')
}
.productComponent {
  transform: translateY(25%);
  box-shadow: 0px 4px 4px 0px #00000040;
  border-radius: 10px;
}
</style>
