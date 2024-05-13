<template>
  <div class="containerCardEmpty">
    <div class="contentEmpty">
      <h3>This product is unavailable to show</h3>
      <button @click="fetchNextProduct" class="buttonEmptyProduct">Next Product</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "EmptyProduct",
  methods: {
    fetchNextProduct() {
      this.$emit("fetch-next-product");
    },
  },
};
</script>

<style>
@import "../assets/style/EmptyProduct.css";
</style>
