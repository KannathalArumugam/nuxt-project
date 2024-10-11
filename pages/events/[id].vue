<script setup>
const nuxt = useNuxtApp();
console.log(nuxt);
</script>

<template>
  <div>Event id : {{ $route.params.id }}</div>

  <div><NuxtLink to="/">Home Page</NuxtLink></div>
  <div><NuxtLink to="/events">Events Page</NuxtLink></div>
  <div><NuxtLink to="/events/profile">Profile Page</NuxtLink></div>
</template>
