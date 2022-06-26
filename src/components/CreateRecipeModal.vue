<template>
  <div class="container mt-5">
    <div>
      <b-button v-b-modal.modal-center>Create New Recipe</b-button>
      <b-modal id="modal-center" centered title="Vue Bootstrap Centered Modal Example">
        <div class="my-4">
          <h1 class="title">Register</h1>
          <b-form @submit.prevent="onRegister" @reset.prevent="onReset">
            <b-form-group id="input-group-title" label-cols-sm="3" label="Title:" label-for="title">
              <b-form-input id="title" v-model="$v.form.title.$model" type="text" :state="validateState('title')">
              </b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.title.required">
                Title is required
              </b-form-invalid-feedback>
              <b-form-invalid-feedback v-else-if="!$v.form.title.length">
                Title length should be at least 2 characters long
              </b-form-invalid-feedback>
              <b-form-invalid-feedback v-if="!$v.form.title.alpha">
                Title alpha
              </b-form-invalid-feedback>
            </b-form-group>

            <b-form-group id="input-group-readyInMinutes" label-cols-sm="3" label="Preperation time in minutes:"
              label-for="readyInMinutes">
              <b-form-input id="readyInMinutes" v-model="$v.form.readyInMinutes.$model" type="text"
                :state="validateState('readyInMinutes')"></b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.required">
                Preperation time is required
              </b-form-invalid-feedback>
              <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.numeric">
                Preperation time is numeric
              </b-form-invalid-feedback>
            </b-form-group>

            <b-form-group id="input-group-imageUrl" label-cols-sm="3" label="Image URL:" label-for="imageUrl">
              <b-form-input id="imageUrl" v-model="$v.form.imageUrl.$model" type="text"
                :state="validateState('imageUrl')"></b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.imageUrl.required">
                Image url is required
              </b-form-invalid-feedback>
              <b-form-invalid-feedback v-if="!$v.form.imageUrl.url">
                Image Url should be url
              </b-form-invalid-feedback>
            </b-form-group>

            <b-form-group id="input-group-popularity" label-cols-sm="3" label="Num of Likes:" label-for="popularity">
              <b-form-input id="popularity" v-model="$v.form.popularity.$model" type="text"
                :state="validateState('popularity')"></b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.popularity.required">
                Popularity is required
              </b-form-invalid-feedback>
              <b-form-invalid-feedback v-if="!$v.form.popularity.numeric">
                Popularity should be numeric
              </b-form-invalid-feedback>
            </b-form-group>

            <b-form-group label="Vegan:" v-slot="{ ariaDescribedby }">
              <b-form-radio id="vegan" v-model="selected" :aria-describedby="ariaDescribedby" name="some-radios"
                value=true>
              </b-form-radio>
            </b-form-group>

            <b-form-group label="Vegetarian:" v-slot="{ ariaDescribedby }">
              <b-form-radio id="vegetarian" v-model="selected" :aria-describedby="ariaDescribedby" name="some-radios"
                value=true>
              </b-form-radio>
            </b-form-group>

            <b-form-group label="Gluten Free:" v-slot="{ ariaDescribedby }">
              <b-form-radio id="glutenFree" v-model="selected" :aria-describedby="ariaDescribedby" name="some-radios"
                value=true>
              </b-form-radio>
            </b-form-group>

            <b-form-group id="input-group-ingredients" label-cols-sm="3" label="Ingredients:" label-for="ingredients">
              <b-form-input id="ingredients" v-model="$v.form.ingredients.$model" type="text"
                :state="validateState('ingredients')"></b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.ingredients.required">
                ingredients is required
              </b-form-invalid-feedback>
              <b-form-invalid-feedback v-if="!$v.form.ingredients.alphaNum">
                ingredients alphaNum
              </b-form-invalid-feedback>
            </b-form-group>

            <b-form-group id="input-group-instructions" label-cols-sm="3" label="Instructions:" label-for="instructions">
              <b-form-input id="instructions" v-model="$v.form.instructions.$model" type="text"
                :state="validateState('instructions')"></b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.instructions.required">
                instructions is required
              </b-form-invalid-feedback>
              <b-form-invalid-feedback v-if="!$v.form.instructions.alphaNum">
                instructions alphaNum
              </b-form-invalid-feedback>
            </b-form-group>


            <b-form-group id="input-group-numOfServings" label-cols-sm="3" label="Num of Servings:" label-for="numOfServings">
              <b-form-input id="numOfServings" v-model="$v.form.popularity.$model" type="text"
                :state="validateState('numOfServings')"></b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.numOfServings.required">
                numOfServings is required
              </b-form-invalid-feedback>
              <b-form-invalid-feedback v-if="!$v.form.numOfServings.numeric">
                numOfServings should be numeric
              </b-form-invalid-feedback>
            </b-form-group>


            

            <b-form-group id="input-group-email" label-cols-sm="3" label="Email:" label-for="email">
              <b-form-input id="email" type="text" v-model="$v.form.email.$model" :state="validateState('email')">
              </b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.email.required">
                Email is required
              </b-form-invalid-feedback>
              <b-form-invalid-feedback v-else-if="!$v.form.email.email">
                The email is invalid
              </b-form-invalid-feedback>
            </b-form-group>

            <b-button type="reset" variant="danger">Reset</b-button>
            <b-button type="submit" variant="primary" style="width:250px;" class="ml-5 w-75">Register
            </b-button>
            <div class="mt-2">
              You have an account already?
              <router-link to="login"> Log in here</router-link>
            </div>
          </b-form>
          <b-alert class="mt-2" v-if="form.submitError" variant="warning" dismissible show>
            Register failed: {{ form.submitError }}
          </b-alert>
          <!-- <b-card class="mt-3 md-3" header="Form Data Result">
      <pre class="m-0"><strong>form:</strong> {{ form }}</pre>
      <pre class="m-0"><strong>$v.form:</strong> {{ $v.form }}</pre>
    </b-card> -->
        </div>
      </b-modal>
    </div>
  </div>
</template>
<script>
import {
  required,
  minLength,
  maxLength,
  alpha,
  numeric,
  alphaNum,
  email,
  url
} from "vuelidate/lib/validators";

export default {
  name: "Register",
  data() {
    return {
      form: {
        title: "",
        readyInMinutes: "",
        imageUrl: "",
        popularity: "",
        vegan: false,
        vegetarian: false,
        glutenFree: false,
        ingredients: "",
        instructions: "",
        numOfServings:""
      },
      countries: [{ value: null, text: "", disabled: true }],
      errors: [],
      validated: false
    };
  },
  validations: {
    form: {
      title: {
        required,
        length: (u) => minLength(2)(u),
        alpha
      },
      readyInMinutes: {
        required,
        numeric
      },
      imageUrl: {
        required,
        url
      },
      popularity: {
        required,
        numeric
      },
      ingredients: {
        required,
        alphaNum
      },
      instructions: {
        required,
        alphaNum
      },
      numOfServings: {
        required,
        numeric
      }
    }
  },
  mounted() {
    // console.log("mounted");
    this.countries.push(...countries);
    // console.log($v);
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async CreateRecipe() {
      try {
        const response = await this.axios.post(
          // "https://test-for-3-2.herokuapp.com/user/Register",
          this.$root.store.server_domain + "/users/myRecipes",

          {
            title: this.form.title,
            readyInMinutes: this.form.readyInMinutes,
            imageUrl: this.form.imageUrl,
            popularity: this.form.popularity,
            vegan: this.form.vegan,
            vegetarian: this.form.vegetarian,
            glutenFree: this.form.glutenFree,
            ingredients: this.form.ingredients,
            instructions: this.form.instructions,
            numOfServings: this.form.numOfServings
          }
        );
        this.$router.push("/users/myRecipes");
        // console.log(response);
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onRegister() {
      // console.log("register method called");
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      // console.log("register method go");
      this.CreateRecipe();
    },
    onReset() {
      this.form = {
        username: "",
        firstName: "",
        lastName: "",
        country: null,
        password: "",
        confirmedPassword: "",
        email: ""
      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    }
  }
};
</script>