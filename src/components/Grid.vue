<template>
  <b-container class="container">
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <div v-if="recipeTemplate==='ourDB'">
    <b-row align-h="around" v-for="i in Math.ceil(recipes.length / 5)" :key="i">
      <b-col  v-for="r in recipes.slice((i - 1) * 5, i * 5)" :key="r.recipe_id">
        <!-- <RecipePreview class="recipePreview" :recipe="r" @click.native='saveSeen(r.id)'/> -->
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
    </div>
    <div v-else>
    <b-row align-h="around" v-for="i in Math.ceil(recipes.length / 5)" :key="i">
      <b-col v-for="r in recipes.slice((i - 1) * 5, i * 5)" :key="r.id">
        <!-- <RecipePreview class="recipePreview" :recipe="r" @click.native='saveSeen(r.id)'/> -->
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>

    </div>
    
  </b-container>
</template>
<!-- 
  <div class="row" v-for="i in Math.ceil(items.length / 5)">
    <span v-for="item in items.slice((i - 1) * 5, i * 5)">
      {{item}}
    </span>
  </div> -->

<script>
import RecipePreview from "./RecipePreview";
export default {
  name: "Grid",
  components: {
    RecipePreview
  },
  props: {
    title: {
      type: String,
      required: true
    },
    recipes:{
      type: Array,
      required: true
    },
    recipeTemplate:{
      type: String,
      required: true
    }

  },
  data() {
    return {
      // recipes: [] // not needed - we get in props
    };
  },
mounted() {
    // this.updateRecipes();
  },
  methods: {
    // async updateRecipes() {
    //   try {
    //     const response = await this.axios.get(
    //       this.$root.store.server_domain + "/recipes/random",
    //       // "http://localhost:3000/recipes/random",
    //     );

    //     console.log(response);
    //     const recipes = response.data.recipes;
    //     this.recipes = [];
    //     this.recipes.push(...recipes);
    //     // console.log(this.recipes);
    //   } catch (error) {
    //     console.log(error);
    //   }
    // },
    async saveSeen(recipeId) {
          //check if user connected
          if(this.$root.store.username){
            //send post request to save lastSeen
            const response = await this.axios.post(
            this.$root.store.server_domain +"/users/lastWatched",
            {
                //the recipe id in body req
                recipeId : recipeId
            },
        );
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  width: 100%;
}
</style>
