<script setup>
import IconMonospace from "./icons/Monospace.vue";
</script>

<template>
  <header>
    <nav class="container">
      <a href="/">
        <IconMonospace />
      </a>
        <router-link to="/animals">Animals</router-link> |
        <router-link to="/about">About</router-link>
      <span class="header__description">
        PetPong Application powered by <span class="accent">RunX</span>
      </span>
    </nav>
  </header>
</template>
