<script setup>
import { RouterLink } from "vue-router";
</script>

<template>
  <main class="error">
    <section>
      <div class="container">
        <h1>About page.</h1>
        <p>
          <RouterLink to="/">Click here</RouterLink> to head back to the safety of the homepage.
        </p>
      </div>
    </section>
  </main>
</template>