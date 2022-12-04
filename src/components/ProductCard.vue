<template>
  <div :class="cardClass">
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
          <button class="btn-buy">
            <a href="'product/${this.count}'">{{ 'Buy Now' }}</a>
          </button>
          <button class="btn-next">
            <a class="btn-next-product" v-on:click="addCount">{{isFetching ? 'Please wait...' : 'Next Product'}}</a>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import { reactive } from "vue";
export default reactive({
  name: "ProductCard",
  props: {
    msg: String,
  },
  data() {
    return {
      products: [],
      gambar_default: "",
      count: 1,
      isFetching: false,
      cardClass: []
    };
  },

  async mounted() {
    try {
      this.isFetching = true
      const response = await this.fetchAPI(this.count)
      const { data: products } = response
      const { id: count, category } = products
      
      this.products = products
      this.count = count
      this.category = category

      this.getBgStyle(this.category)

      this.isFetching = false
    } catch (error) {
      this.isFetching = false
      console.log(error)
    }
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
    getBgStyle(category){
      switch (category) {
        case "men's clothing":
          this.cardClass = 'container-men'
          break
        case "women's clothing":
          this.cardClass = 'container-women'
          break
        default:
          this.cardClass = 'container-women'
          break
        case "jewelery":
          this.cardClass = 'container-unavailable'
          break
          case "electronics":
          this.cardClass = 'container-unavailable'
          break
      }
    },
    async addCount() {
      if(!this.isFetching){
        this.isFetching = true
        if (
          this.products.category == "men's clothing" ||
          this.products.category == "women's clothing"
        ) {
          if (this.count == 20) {
            this.count = 1;
          } else {
            this.count++;
          }
        } else {
          this.count++;
        }
        
        const response = await this.fetchAPI(this.count)
        const { data: products } = response
        const { id: count, category } = products
        
        this.products = products
        this.count = count
        this.category = category

        this.getBgStyle(this.category)
        
        this.isFetching = false
      }
    },
    async fetchAPI(count){
      return await axios.get("https://fakestoreapi.com/products/" + count)
    },
  },
});
</script>
