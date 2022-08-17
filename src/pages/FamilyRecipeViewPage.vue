<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1>
        <h3>Recipe Owner: {{ recipe.recipeOwner }}</h3>
        <img :src="recipe.imageUrl" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div>Preperation time in {{ recipe.preparationTime }}</div>
            </div>
        <div>Ingredients: {{ recipe.ingredients }}</div>
        <div>Instructions: {{ recipe.instructions }}</div>

          </div>
          </div>
      </div>
      <!-- <pre>
      {{ $route.params }}
      {{ recipe }}
    </pre
      > -->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      recipe: null
    };
  },
  async created() {
    try {
      let response;
      // response = this.$route.params.response;

      try {
        this.recipe = this.$route.params.recipe;

        // console.log("response.status", response.status);
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
      console.log("the response is: ",this.recipe);

      let {
        id,
        title,
        recipeOwner,
        readyInMinutes,
        image,
        ingredients,
        instructions,
        user_id
      } = this.recipe;

      let _instructions = instructions
        .map((fstep) => {
          fstep.steps[0].step = fstep.name + fstep.steps[0].step;
          return fstep.steps;
        })
        .reduce((a, b) => [...a, ...b], []);

      let _recipe = {
        id,
        title,
        recipeOwner,
        readyInMinutes,
        image,
        ingredients,
        instructions,
        user_id
      };
      console.log(_recipe);

      this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
/* .recipe-header{

} */
</style>
