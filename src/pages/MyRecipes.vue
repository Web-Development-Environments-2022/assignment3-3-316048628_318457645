<template>
<div>
    <h3>
      My Recipes
    </h3>
    <Grid title="My Recipes" class="MyRecipes center" :recipes="recipes" :recipeTemplate="recipeTemplate"></Grid>
</div>
</template>

<script>
import Grid from "../components/Grid";
export default {
  name: "Favorites",
  components: {
    Grid
  },
  data() {
    return {
      recipes: [],
      recipeTemplate:"ourDB"
    };
  },
  async mounted() {
    try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/myRecipes"
        );
        console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        // return response;
      }
      catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      } 
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
