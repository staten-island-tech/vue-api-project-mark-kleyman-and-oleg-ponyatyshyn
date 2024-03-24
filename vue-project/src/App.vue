<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HomeView from './views/HomeView.vue';
</script>

<template>
  <header>
    <div class="wrapper">
      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
        <RouterLink to="/fart">skibidi</RouterLink>
      </nav>
      <RouterView id="fart"/>
    </div>
  </header>

</template>

<style scoped>
header {
  align-items: center;
  line-height: 1.5;
  max-height: 100vh;
}
#fart{
  width: 60%;
}
.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 1.5rem;
  margin-bottom: 1.5rem;
  align-items: center;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
  align-items: center;
}

nav a:first-of-type {
  border: 0;
}
  .wrapper {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    justify-content: center;
  }

</style>