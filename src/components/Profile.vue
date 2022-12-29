<script setup>
import {
  reactive,
  ref,
  computed,
  onMounted,
  onUpdated,
  onUnmounted,
} from "vue";

import UserInfo from "./UserInfo.vue";

const searchInput = ref("");

const state = reactive({
  login: "",
  name: "",
  bio: "",
  company: "",
  avatar_url: "",
  repos: [],
});

async function fetchGithubUser(ev) {
  ev.preventDefault();
  const res = await fetch(`https://api.github.com/users/${searchInput.value}`);
  const { login, name, bio, company, avatar_url } = await res.json();

  state.login = login;
  state.name = name;
  state.bio = bio;
  state.company = company;
  state.avatar_url = avatar_url;
  state.repos = [];

  fetchUserRepositories();
}

async function fetchUserRepositories() {
  const res = await fetch(`https://api.github.com/users/${state.login}/repos`);
  const repos = await res.json();
  state.repos = repos;
}

const reposCountMessage = computed(() => {
  return state.repos.length > 0
    ? `${state.name} has ${state.repos.length} public repositories`
    : `${state.name} has no public repositories`;
});

onMounted(() => {
  console.log("The component has been mounted");
});

onUpdated(() => {
  console.log("The component has been updated");
});

onUnmounted(() => {
  console.log("The component has been unmounted");
});
</script>

<template>
  <h1>GitHub User Data</h1>
  <p>
    Searching for:
    <strong>https://api.github.com/users/{{ searchInput }}</strong>
  </p>
  <form @submit="fetchGithubUser">
    <input type="text" v-model="searchInput" />
    <button>Load User</button>
  </form>
  <div v-if="state.login !== ''">
    <UserInfo
      :login="state.login"
      :name="state.login"
      :company="state.company"
      :bio="state.bio"
      :avatar_url="state.avatar"
    />
  </div>

  <h2>
    {{ reposCountMessage }}
  </h2>

  <section v-if="state.repos.length > 0">
    <article v-for="repo of state.repos">
      <h3>{{ repo.full_name }}</h3>
      <p>{{ repo.description }}</p>
      <a :href="repo.html_url" target="_blank">Watch on GitHub</a>
    </article>
  </section>
</template>

// ...

<style scoped>
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

section {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-top: 1rem;
}
article {
  border: 1px solid #f64348;
  border-radius: 0.25rem;
  padding: 0 1rem 1rem;
}
</style>
