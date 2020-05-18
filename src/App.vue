<template>
  <div class="container">
    <div class="row justify-content-center">
      <h1 class="title">Card Memory Game</h1>
    </div>
    <hr />
    <div class="row d-flex justify-content-justify">
      <cards></cards>
    </div>
  </div>
</template>

<script>
import Cards from "./components/Cards.vue";

export default {
  components: {
    Cards
  }
};
</script>

<style>
.title {
  font-family: "Righteous", cursive;
}
</style>
