<template>
  <div class="card">
    <img src="http" alt="Card Image" />
    <div class="card-body">
      <h2>prod</h2>
      <p></p>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardArticle",
  props: ["produk"],
};
</script>

<style>
.card {
  border: 1px solid #ccc;
  border-radius: 5px;
  margin: 10px;
  padding: 10px;
  width: 300px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

img {
  width: 100%;
  max-height: 150px;
  object-fit: cover;
}

h2 {
  font-size: 1.5rem;
  margin: 0;
}

p {
  font-size: 1rem;
  margin: 0;
  color: #666;
}
</style>
