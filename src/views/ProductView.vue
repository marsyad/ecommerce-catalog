<template>
  <div class="container">
    <div class="cards">
      <div class="row">
        <div class="product-img">
          <img v-bind:src="products.image" width="305" alt="" />
        </div>
        <div class="layout-product-description">
          <h1 class="product-name">
            {{ products.title }}
          </h1>
          <p class="product-category">{{ products.category }}</p>
          <div class="ratingbar">
            <span>{{ products.rating.rate }}/5</span>
            <img src="assets/img/circle.png" />
            <img src="assets/img/circle.png" />
            <img src="assets/img/circle.png" />
            <img src="assets/img/circle-border.png" />
            <img src="assets/img/circle-border.png" />
          </div>
          <hr class="line" />
          <p class="product-description">
            {{ products.description }}
          </p>
          <hr class="line" />
          <p class="product-price">${{ products.price }}</p>
          <button class="btn-buy"><a href="">Buy Now</a></button>
          <button class="btn-next">
            <a href="`/product/`">Next Product</a>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "ProductView",
  props: {
    msg: String,
  },
  data() {
    return {
      products: [],
      gambar_default: "",
    };
  },

  mounted() {
    axios
      .get("https://fakestoreapi.com/products/", {
        params: {
          id: this.$route.params.id,
        },
      })
      .then((response) => {
        this.products = response.data.data;
      })
      .catch((error) => {
        console.log(error);
      });
  },
  methods: {
    changeImage(urlImage) {
      this.gambar_default = urlImage;
    },
    setDataPicture(data) {
      // replace object productDetails dengan data dari API
      this.products = data;
      // replace value gambar default dengan data dari API (galleries)
      this.gambar_default = data.images;
    },
  },
};
</script>
