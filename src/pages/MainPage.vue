<template>


  <b-container class="container">
    <b-row>
      <h3 v-if="!$root.store.username" >Hello Guest</h3>
      </b-row>
      <b-row>
    <b-col>
    <RecipePreviewList title="Randome Recipes" class="RandomRecipes center" :recipes="randomRecipes" />
    <div class="moreBtn">
  
      <b-button @click="GetRandom">More</b-button>
    </div>
    </b-col>
    <b-col>
    <!-- <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this</router-link> -->
    <!-- {{ !$root.store.username }} -->
      <RecipePreviewList v-if="$root.store.username"
      title="Last Viewed Recipes"
      :class="{
        RandomRecipes: true,
        blur: !$root.store.username,
        center: true
      }"
      disabled
      :recipes="lastViewedRecipes"
    >  </RecipePreviewList>
    <Login v-else>

    </Login>
    </b-col>
    <div
      style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
    </div>
    </b-row>
</b-container>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
import Login from "../components/Login";

export default {
  data() {
    return {
      randomRecipes : [],
      lastViewedRecipes : []

    }

  },
  components: {
    RecipePreviewList,
    Login
},
mounted()
{     
  // this.GetRandom();
  // this.GetLastWathced();
}
,
methods:
{
  async GetRandom(){
  try {
        // console.log(this.$root.store.server_domain);
        const response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/random",
        );
        console.log(response);
        const recipes = response.data.recipes;
        // this.randomRecipes = recipes;
        this.randomRecipes = [];
        this.randomRecipes.push(...recipes);

      } catch (error) {
        console.log(error);
      }
  },
  async GetLastWathced(){
    
    if(this.$root.store.username){
      try {
        console.log("last watched");
          const response = await this.axios.get(
            this.$root.store.server_domain + "/users/lastWatched",
          );
          console.log("the response of last wathced",response);
          const recipes = response.data;
          this.lastViewedRecipes = [];
          this.lastViewedRecipes.push(...recipes);
          console.log("lastViewedRecipes",response);

        } catch (error) {
          console.log(error);
        }
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
#h3 {
  text-align: left;
}

.moreBtn {
  align-items: center;
  margin-left: 40%;
}

</style>
