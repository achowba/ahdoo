<script setup>
import { RouterLink } from 'vue-router';
</script>

<template>
  <header>
    <nav class="container">
      <div class="branding">
        <img src="../assets/img/logo.png" alt="Logo">
        <h1>Ahdoo</h1>
      </div>
      <ul class="nav-routes">
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </ul>
    </nav>
  </header>
</template>

<style lang="scss" scoped>
header {
  background-color: #f1f1f1;

  nav {
    align-items: center;
    display: flex;
    padding: 25px 16px;

    .branding {
      align-items: center;
      display: flex;
      gap: 8px;

      img {
        max-width: 70px;
      }

      h1 {
        font-size: 24px;
      }
    }

    .nav-routes {
      display: flex;
      flex: 1;
      gap: 12px;
      justify-content: flex-end;
      list-style: none;

      a {
        color: inherit;
        text-decoration: none;
      }
    }
  }
}
</style>
