<template>
<div>
  <b-container>
  <router-link
    :to="{ name: 'recipe', params: { recipeId: recipe.id } }"
    class="recipe-preview"
  >
  <b-row>
 <div class="recipe-body">
      <img :src="recipe.image" class="recipe-image" />
    </div>
  </b-row>
   <b-row>
<div class="recipe-footer">
      <div :title="recipe.title" class="recipe-title">
        {{ recipe.title }}
      </div>
      <ul class="recipe-overview">
        <li> <b-icon icon="clock" ></b-icon> {{ recipe.readyInMinutes }}</li>
        <li> <b-icon icon="hand-thumbs-up" ></b-icon> {{ recipe.aggregateLikes }}</li>
        <li v-if="seen">already seen : <b-icon icon="eye-slash" ></b-icon></li>
        <li v-else>already seen : <b-icon icon="eye" ></b-icon></li>

      </ul>
      <div>
      </div>
    </div>
   </b-row>
    
  </router-link>
  </b-container>
    <b-button id="favbutton" v-if="$root.store.username" @click="AddToFav"><b-icon v-if="already_in_fav" icon="star-fill" style="color:yellow;"></b-icon> <b-icon icon="star-fill" v-else style="color:white;"></b-icon> </b-button>

    <!-- <button :class="already_in_fav" v-if="$root.store.username"  @click="AddToFav">Add To Favorite</button> -->
  </div>
 
</template>

<script>
import { Console } from 'console';

export default {
  mounted() {
    this.CheckIfAlreadySeen();
    this.CheckIfAlreadyInFav();
  },
  data() {
    return {
      // image_load: false,
      already_in_fav : false,
      seen : false
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true
    },



    // id: {
    //   type: Number,
    //   required: true
    // },
    // title: {
    //   type: String,
    //   required: true
    // },
    // readyInMinutes: {
    //   type: Number,
    //   required: true
    // },
    // image: {
    //   type: String,
    //   required: true
    // },
    // aggregateLikes: {
    //   type: Number,
    //   required: false,
    //   default() {
    //     return undefined;
    //   }
    // }
  },
  methods:
  {
  async AddToFav(){

      this.already_in_fav = "true";
      //send post request to save favorite recipe
      if(this.recipe != 'undefined')
      // console.log(this.$root.store.server_domain);
      // console.log(this.recipe);
      {
      const response = await this.axios.post(
      this.$root.store.server_domain +"/users/favorites",
      {
          //the recipe id in body req
          recipe_id : this.recipe.id
      },
  );
      }
      
  },
  async CheckIfAlreadyInFav(){
    try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/favorites",
          // "http://localhost:3000/recipes/random",
        );
        console.log("the response in CheckIfAlreadyInFav", response);
        const recipes = response.data;
        const recipesId = recipes.map( r => r.id);
        if(recipesId.includes(this.recipe.id))
          {
              console.log(this.recipe,"recipe is in fave list");
              this.already_in_fav = true;
          }
        // console.log(this.recipes);
      } catch (error) {
        console.log("error in CheckIfAlreadyInFav" ,error);
      }
  },

  async CheckIfAlreadySeen(){
    try {
        console.log("in CheckIfAlreadySeen, recipe id:", this.recipe.id);
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/lastWatched/"+ this.recipe.id );
        console.log("the response from /users/lastWatched/recipeId: ",response);
        const recipes = response.data;
        if(recipes === [])
        {
          console.log("seen is true");
          this.seen =true;
        }

  }
  catch(error) {
      console.log("error in CheckIfAlreadySeen",error);

  }

  }
}}
;
</script>

<style scoped>
.recipe-preview {
  display: inline-block;
  width: 90%;
  height: 100%;
  position: relative;
  margin: 10px 10px;
}
.recipe-preview > .recipe-body {
  width: 100%;
  height: 200px;
  position: relative;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 98%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

.recipe-preview .recipe-footer {
  width: 100%;
  height: 50%;
  overflow: hidden;
}

.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
}



</style>
