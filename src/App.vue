<template>
  <div id="app">
<<<<<<< HEAD
    <!-- <div id="nav"> -->
    <!-- <router-link :to="{ name: 'main' }">Vue Recipes</router-link> |
      <router-link :to="{ name: 'search' }">Search</router-link> |
      <router-link :to="{ name: 'about' }">About</router-link> | -->

    <!-- <span v-if="!$root.store.username">
        <router-link :to="{ name: 'register' }">Register</router-link> |
        <router-link :to="{ name: 'login' }">Login</router-link> |
        Hello Guest
      </span>
      <span v-else>
        {{ $root.store.username }}: <button @click="Logout">Logout</button> |
        {{ $root.store.username }}: <CreateRecipeModal title="Create Recipe" /> |
        {{ $root.store.username }}: Hello , {{ $root.store.username }} |
      </span> -->
    <!-- </div> -->

    <div id="nav">
      <b-navbar class="navbar navbar-dark bg-white" toggleable="lg" type="white" variant="info">
      <b-navbar-brand :to="{ name: 'main' }">Main </b-navbar-brand>
      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
        <b-nav-item :to="{ name: 'search' }">Search</b-nav-item>
          <b-nav-item :to="{ name: 'about' }">About</b-nav-item>
        </b-navbar-nav>
        <b-navbar-nav v-if="$root.store.username">
          <b-nav-item-dropdown>
          <template #button-content>Personal </template>
          <b-dropdown-item :to="{ name: 'favorites' }">My Favorites</b-dropdown-item>
          <b-dropdown-item :to="{ name: 'myRecipes' }">My Recipes</b-dropdown-item>
          <b-dropdown-item :to="{ name: 'familyRecipes' }">My Family Recipes</b-dropdown-item>
        </b-nav-item-dropdown>
          <b-nav-item ref="CreateRecipeModal" @click="OpenModal" v-b-modal.modal-center>Create Recipe
            <CreateRecipeModal v-if="showModal"/>
          </b-nav-item>
        </b-navbar-nav>
        <b-navbar-nav class="ml-auto" v-if="$root.store.username">
                  <b-nav-item-dropdown right>
                  <template #button-content>{{$root.store.username}}</template>
                  <b-dropdown-item href="#" @click="Logout">Log Out</b-dropdown-item>
        </b-nav-item-dropdown>
        </b-navbar-nav>
        <b-navbar-nav class="ml-auto" v-else>
        <b-navbar-brand> Hello guest </b-navbar-brand>
          <b-nav-item :to="{ name: 'login' }">Login</b-nav-item>
          <b-nav-item :to="{ name: 'register' }">Register</b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
=======
    <div id="nav">
      <router-link :to="{ name: 'main' }">Vue Recipes</router-link>|
      <router-link :to="{ name: 'search' }">Search</router-link>|
      <router-link :to="{ name: 'favorites' }">My Favorites</router-link>
      {{ !$root.store.username }}
      <span v-if="!$root.store.username">
        Guest:
        <router-link :to="{ name: 'register' }">Register</router-link>|
        <router-link :to="{ name: 'login' }">Login</router-link>|
      </span>
      <span v-else>
        {{ $root.store.username }}: <button @click="Logout">Logout</button>|
        {{ $root.store.username }}: <CreateRecipeModal title="Create Recipe" />|
        {{ $root.store.username }}: <router-link :to="{ name: 'favorites' }">My Favorites</router-link>
      </span>
>>>>>>> 42daaf9196c11906108f99e0f2f52b7ea354f204
    </div>
    <router-view />
  </div>
</template>

<script>
import CreateRecipeModal from "./components/CreateRecipeModal.vue";

export default {
    name: "App",
    data(){
        return {
          showModal: false
        }
    },
    methods: {
        Logout() {
            this.$root.store.logout();
            this.$root.toast("Logout", "User logged out successfully", "success");
            this.$router.push("/").catch(() => {
                this.$forceUpdate();
            });
        },
        OpenModal() {

          this.showModal = true

        }
    },
    components: { 
      CreateRecipeModal 
    }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
