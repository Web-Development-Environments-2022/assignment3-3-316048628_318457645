<template>
  <b-container>
    <h3>
      Family Recipes
    </h3>
    <b-row>
      <b-col v-for="r in recipes" :key="r.recipe_id">
        <FamilyRecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import FamilyRecipePreview from "../components/FamilyRecipePreview.vue";
export default {
  name: "Family",
  components: {
    FamilyRecipePreview
  },
  data() {
    return {
      recipes: []
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/familyRecipes",
          // "http://localhost:3000/recipes/random",
        );

        console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
