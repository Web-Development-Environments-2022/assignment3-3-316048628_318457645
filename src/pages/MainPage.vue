<template>
  <div class="container">
    <h1 class="title">Main Page</h1>
    <RecipePreviewList title="Randome Recipes" class="RandomRecipes center" :recipes="randomRecipes" />
    <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this</router-link>
    {{ !$root.store.username }}
    <RecipePreviewList
      title="Last Viewed Recipes"
      :class="{
        RandomRecipes: true,
        blur: !$root.store.username,
        center: true
      }"
      disabled
      :recipes="lastViewedRecipes"
    ></RecipePreviewList>
    <div
      style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
    </div>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
export default {
  data() {
    return {
      randomRecipes : [],
      lastViewedRecipes : []

    }

  },
  components: {
    RecipePreviewList,
},
async mounted()
{     
  try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/random",
        );
        console.log(response);
        const recipes = response.data.recipes;
        this.randomRecipes = recipes;
      } catch (error) {
        console.log(error);
      }

    if($root.store.username){
      try {
          const response = await this.axios.get(
            this.$root.store.server_domain + "/users/lastWatched",
          );
          console.log(response);
          const recipes = response.data.recipes;
          this.lastViewedRecipes = recipes;
        } catch (error) {
          console.log(error);
        }
      
    
  }
}
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
</style>
