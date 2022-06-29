<template>
<div>
  <div class="container">
    <h1 class="title">Search Page</h1>

    <b-form @submit.prevent="onSearch" @reset.prevent="onReset">
      <b-form-group id="input-group-queryS" label-cols-sm="3" label="Query to search:" label-for="queryS">
        <b-form-input id="queryS" v-model="$v.form.queryS.$model" type="text" :state="validateState('queryS')">
        </b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.queryS.length">
          Query length should be at least 2 characters long
        </b-form-invalid-feedback>
      </b-form-group>
      <b-form-group
        id="input-group-numOfRes"
        label-cols-sm="3"
        label="Num Of Results:"
        label-for="numOfRes"
      >
      <b-form-select id="numOfRes" v-model="myNumOfRes" :options="options"></b-form-select>
      </b-form-group>
       <b-form-group
        id="input-group-diet"
        label-cols-sm="3"
        label="Diet:"
        label-for="diet"
      >
      <b-form-select id="diet" v-model="myDiet" :options="dietOptions"></b-form-select>
      </b-form-group>

      
       <b-form-group
        id="input-group-intolerance"
        label-cols-sm="3"
        label="Intolerance:"
        label-for="intolerance"
      >
      <b-form-select id="intolerance" v-model="myIntolerance" :options="intolerances"></b-form-select>
      </b-form-group>

      <b-button type="reset" variant="danger">Reset</b-button>
      <b-button type="submit" variant="primary" style="width:250px;" class="ml-5 w-75">Search</b-button>
    </b-form>
  </div>
   
    <div id="results-container">
      <b-container>
    <h3>
    Search Results
    </h3>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container>

    </div>
  </div>
</template>

<script>
import RecipePreview from "../components/RecipePreview.vue";

import dietOptions from "../assets/dietOptions";
import intolerances from "../assets/intolerances";

import {
  minLength,
} from "vuelidate/lib/validators";

export default {
  name: "Search",
    components: {
    RecipePreview
  },
  data() {
    return {
      recipes: [],
      form: {
        queryS: "",
        numOfRes: 5,
        cuisine: null,
        diet: "",
        intolerance: "",
        submitError: undefined
      },
      dietOptions: [{ value: null, text: "", disabled: true }],
      intolerances: [{ value: null, text: "", disabled: true }],

      // selected: 5,
      options: [
        { value: 5, text: '5 results' },
        { value: 10, text: '10 results' },
        { value: 15, text: '15 results' },
      ],

      errors: [],
      validated: false
    };
  },
  computed:{
    myNumOfRes: {get(){
      return this.form.numOfRes
    },
    set(value){
      if(this.numOfRes!==5)
      {
        this.form.numOfRes = value
      }
    },
  },

  myDiet: {get(){
      return this.form.diet
    },
    set(value){
      if(this.diet!=="")
      {
        this.form.diet = value
      }
    },
  },

myIntolerance: {get(){
      return this.form.intolerance
    },
    set(value){
      if(this.intolerance!=="")
      {
        this.form.intolerance = value
      }
    },
  },

  },
  validations: {
    form: {
      queryS: {
        length: (u) => minLength(2)(u),
      },
    }
  },
  mounted() {
    // console.log("mounted");
    this.dietOptions.push(...dietOptions);
    this.intolerances.push(...intolerances);
    // console.log($v);
  },

  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Search() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/search",
          {
            params:
            {
              query: this.form.queryS,
              num: this.form.numOfRes,
              cuisine: this.form.cuisine,
              diet: this.form.diet,
              intolerance: this.form.intolerance,
            }
          }
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
    },
    onSearch() {
      // console.log("register method called");
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      // console.log("register method go");
      this.Search();
    },
    onReset() {
      this.form = {
        queryS: "",
        numOfRes: null,
        cuisine: "",
        diet: "",
        intolerance: "",
      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    }
  }
};

</script>
<style lang="scss" scoped>
.container {
  max-width: 500px;
}
</style>

