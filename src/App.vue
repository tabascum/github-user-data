<script setup>
import { ref } from "vue";
import Profile from "./components/Profile.vue";

const isOpen = ref(true);
</script>

<template>
  <input type="checkbox" v-model="isOpen" />{{ isOpen }}
  <Profile v-if="isOpen" />
</template>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background-color: #1c1a1d;
  color: #e5e5e5;
}

#app {
  font-family: "Ubuntu", sans-serif;
  min-height: 100vh;
  max-width: 40rem;
  margin: 0 auto;
  text-align: center;
}

input,
button {
  max-width: 20rem;
  padding: 0.5rem;
}

input {
  background-color: #1c1a1d;
  border: 1px solid #f64348;
  border-radius: 0.25rem;
  color: #e5e5e5;
  margin: 1rem 1rem 1rem 0;
}

button {
  background-color: #f64348;
  border: unset;
  border-radius: 0.25rem;
  color: #1c1a1d;
  font-size: 1rem;
  font-weight: 700;
  text-transform: uppercase;
}

button:hover {
  cursor: pointer;
  filter: brightness(0.95);
}

a {
  color: #f64348;
}
</style>
