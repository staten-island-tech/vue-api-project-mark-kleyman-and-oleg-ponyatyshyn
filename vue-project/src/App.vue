<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HomeView from './views/HomeView.vue';
</script>

<template>
  <header>
    <div class="wrapper">

      <nav>
        <RouterLink to="/">Crashes</RouterLink>
        <RouterLink to="/fart">SAT</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  text-align: center;
  line-height: 1.5;
  max-height: 100vh;
}

header .wrapper {
  display: flex;
  place-items: flex-start;
  flex-wrap: wrap;
  align-items: center;

}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 1rem;
  display: flex; 
  justify-content: center; 
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
  align-items: center;
  text-align: center;
  margin: 0 auto; 
}


nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}


nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    align-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
    align-items: center;
  }

  nav {
    align-items: center;
    text-align: center;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>