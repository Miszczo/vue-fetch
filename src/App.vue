<script setup lang="ts">
import PlayersListing from './components/PlayersListing.vue';
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <h1>Dummy title</h1>

    <div class="wrapper">
      <nav>
        <RouterLink to="/">Listing</RouterLink>
        <RouterLink to="/about">Another page in future</RouterLink>
      </nav>
    </div>
  </header>
  <RouterView />
</template>