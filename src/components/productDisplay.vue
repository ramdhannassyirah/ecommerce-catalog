<template>
  <div>
    <div class="container" :class="currentCategoryClass">
      <div class="bg">
        <div class="image">
          <img :src="product.image" alt="" />
        </div>
        <div class="deskripsi">
          <h1 class="title" :class="textClass">{{ product.title }}</h1>

          <h4 class="categori">{{ product.category }}</h4>
          <hr />

          <p class="text">{{ product.description }}</p>
          <div class="harga">
            <hr />
            <h1>${{ product.price }}</h1>
          </div>
          <div class="button">
            <button :class="buttonClass">Buy Now</button>
            <button :class="buttonClass" @click="getNextProduct">
              Next Product
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
@import "@/assets/style/main.css";
</style>

<script>
import axios from "axios";

export default {
  data() {
    return {
      products: [],
      categories: [],
      currentCategoryIndex: 0,
      currentIndex: 0,
    };
  },

  computed: {
    product() {
      return this.products[this.currentIndex];
    },

    currentCategoryClass() {
      // Menentukan class CSS berdasarkan kategori saat ini
      const selectedCategory = this.categories[this.currentCategoryIndex];
      if (selectedCategory === "men's clothing") {
        return "category-men";
      } else if (selectedCategory === "women's clothing") {
        return "category-women";
      } else {
        return "category-default";
      }
    },

    buttonClass() {
      // Menentukan class CSS untuk tombol Buy Now berdasarkan kategori saat ini
      const selectedCategory = this.categories[this.currentCategoryIndex];
      if (selectedCategory === "men's clothing") {
        return "button-men";
      } else if (selectedCategory === "women's clothing") {
        return "button-women";
      } else {
        return "button-default";
      }
    },

    textClass() {
      // Menentukan class CSS untuk warna teks berdasarkan kategori saat ini
      const selectedCategory = this.categories[this.currentCategoryIndex];
      if (selectedCategory === "men's clothing") {
        return "text-men";
      } else if (selectedCategory === "women's clothing") {
        return "text-women";
      } else {
        return "text-default";
      }
    },
  },

  methods: {
    async getNextProduct() {
      if (this.currentIndex === this.products.length - 1) {
        // Periksa jika produk terakhir dalam kategori saat ini
        if (this.currentCategoryIndex === this.categories.length - 1) {
          this.currentCategoryIndex = 0; // Jika kategori terakhir, kembali ke kategori pertama
        } else {
          this.currentCategoryIndex++; // Pindah ke kategori berikutnya
        }
        this.fetchProductsByCategory(this.currentCategoryIndex);
      } else {
        this.currentIndex++; // Tampilkan produk berikutnya dalam kategori saat ini
      }
    },

    async fetchProductsByCategory(categoryIndex) {
      const selectedCategory = this.categories[categoryIndex];

      axios
        .get(`https://fakestoreapi.com/products/category/${selectedCategory}`)
        .then((res) => res.data)
        .then((products) => {
          this.products = products;
          this.currentIndex = 0;
        })
        .catch((error) => {
          console.error("Error fetching products:", error);
        });
    },

    async fetchCategories() {
      axios
        .get("https://fakestoreapi.com/products/categories")
        .then((res) => res.data)
        .then((categories) => {
          this.categories = categories;
          this.fetchProductsByCategory(this.currentCategoryIndex);
        })
        .catch((error) => {
          console.error("Error fetching categories:", error);
        });
    },
  },

  mounted() {
    this.fetchCategories();
  },
};
</script>
