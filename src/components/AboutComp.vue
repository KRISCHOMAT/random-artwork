<script setup>
import LogoComp from "./LogoComp.vue";
</script>

<template>
  <div class="wrapper">
    <h1>About</h1>
    <p>
      This App is showing a random artwork of the Art Institute Chicago. It is
      made using their free available API
      {{ "(Application Programming Interface)" }}.
    </p>
    <LogoComp />
    <div class="footer">
      <p>{{ "(c)" }} Christian Grothe</p>
      <a href="https://www.christian-grothe.com">www.christian-grothe.com</a>
    </div>
  </div>
</template>

<style scoped>
a {
  text-decoration: none;
  color: black;
}

.wrapper {
  padding: 50px;
  display: flex;
  align-items: center;
  flex-direction: column;
}

.footer {
  position: absolute;
  margin-left: auto;
  margin-right: auto;
  left: 0;
  right: 0;
  bottom: 20px;
}
</style>
