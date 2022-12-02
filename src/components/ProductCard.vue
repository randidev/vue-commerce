<template>
  <div class="container-men">
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
            <a href="'product/${this.count}'">{{ id }}</a>
          </button>
          <button class="btn-next">
            <a class="btn-next-product" v-on:click="addCount">Next Product</a>
          </button>
          <p>{{ count }}</p>
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
    };
  },

  mounted() {
    axios
      .get("https://fakestoreapi.com/products/" + this.count)
      .then((response) => {
        this.products = response.data;
        this.count = response.data.id;
        this.category = response.data.category;
        //const background = document.getElementsByClassName("container-men");
        if (this.category == "men's clothing") {
          document
            .getElementsByClassName("container-men")
            .classList.add("dark-mode");
        } else if (this.category == "women's clothing") {
          return {
            theme: "container-women",
          };
        }
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

    addCount() {
      if (
        this.products.category == "men's clothing" ||
        this.products.category == "women's clothing"
      ) {
        if (this.count == 20) {
          this.count = 1;
          axios
            .get("https://fakestoreapi.com/products/" + this.count)
            .then((response) => {
              this.products = response.data;
              this.count = response.data.id;
            })
            .catch((error) => {
              console.log(error);
            });

          console.log(this.products.category);
        } else {
          this.count++;
          axios
            .get("https://fakestoreapi.com/products/" + this.count)
            .then((response) => {
              this.products = response.data;
              this.count = response.data.id;
            })
            .catch((error) => {
              console.log(error);
            });
          console.log(this.products.category);
        }
      } else {
        this.count++;
        axios
          .get("https://fakestoreapi.com/products/" + this.count)
          .then((response) => {
            this.products = response.data;
            this.count = response.data.id;
          })
          .catch((error) => {
            console.log(error);
          });
        console.log(this.products.category);
      }
    },
  },
});
</script>
