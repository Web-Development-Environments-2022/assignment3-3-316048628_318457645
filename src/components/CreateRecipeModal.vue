<template>
    <div class="container mt-5">
        <div id="createRecipeModal">
            <!-- <b-button v-b-modal.modal-center>Create New Recipe</b-button> -->
            <b-modal id="modal-center" centered title="Create New Recipe" ok-title="Create Recipe" cancel-title="Reset" @ok.prevent="onCreate" @cancel.prevent="onReset">
                <div class="my-4">
                    <b-form >
                        <b-form-group id="input-group-title" label-cols-sm="3" label="Title:" label-for="title">
                            <b-form-input id="title" v-model="$v.form.title.$model" type="text"
                                :state="validateState('title')">
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

                        <b-form-group id="input-group-readyInMinutes" label-cols-sm="3"
                            label="Preperation time in minutes:" label-for="readyInMinutes">
                            <b-form-input id="readyInMinutes" v-model="$v.form.readyInMinutes.$model" type="text"
                                :state="validateState('readyInMinutes')"></b-form-input>
                            <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.required">
                                Preperation time is required
                            </b-form-invalid-feedback>
                            <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.numeric">
                                Preperation time is numeric
                            </b-form-invalid-feedback>
                        </b-form-group>

                        <b-form-group id="input-group-imageUrl" label-cols-sm="3" label="Image URL:"
                            label-for="imageUrl">
                            <b-form-input id="imageUrl" v-model="$v.form.imageUrl.$model" type="text"
                                :state="validateState('imageUrl')"></b-form-input>
                            <b-form-invalid-feedback v-if="!$v.form.imageUrl.required">
                                Image url is required
                            </b-form-invalid-feedback>
                            <b-form-invalid-feedback v-if="!$v.form.imageUrl.url">
                                Image Url should be url
                            </b-form-invalid-feedback>
                        </b-form-group>

                        <b-form-group id="input-group-popularity" label-cols-sm="3" label="Num of Likes:"
                            label-for="popularity">
                            <b-form-input id="popularity" v-model="$v.form.popularity.$model" type="text"
                                :state="validateState('popularity')"></b-form-input>
                            <b-form-invalid-feedback v-if="!$v.form.popularity.required">
                                Popularity is required
                            </b-form-invalid-feedback>
                            <b-form-invalid-feedback v-if="!$v.form.popularity.numeric">
                                Popularity should be numeric
                            </b-form-invalid-feedback>
                        </b-form-group>
                            <b-form-checkbox id="checkbox-vegan" v-model="veganRes" name="checkbox-1" value=1
                                unchecked-value=0>
                                Vegan
                            </b-form-checkbox>
                            <b-form-checkbox id="vegetarian" v-model="vegetarianRes" name="checkbox-2" value=1
                                unchecked-value=0>
                                Vegetarian
                            </b-form-checkbox>

                            <b-form-checkbox id="glutenFree" v-model="glutenFreeRes" name="checkbox-3" value=1
                                unchecked-value=0>
                                Gluten Free
                            </b-form-checkbox>

                        <b-form-group id="input-group-ingredients" label-cols-sm="3" label="Ingredients:"
                            label-for="ingredients">
                            <b-form-input id="ingredients" v-model="$v.form.ingredients.$model" type="text"
                                :state="validateState('ingredients')"></b-form-input>
                            <b-form-invalid-feedback v-if="!$v.form.ingredients.required">
                                ingredients is required
                            </b-form-invalid-feedback>
                            <!-- <b-form-invalid-feedback v-if="!$v.form.ingredients.alphaNum">
                                ingredients alphaNum
                            </b-form-invalid-feedback> -->
                        </b-form-group>

                        <b-form-group id="input-group-instructions" label-cols-sm="3" label="Instructions:"
                            label-for="instructions">
                            <b-form-input id="instructions" v-model="$v.form.instructions.$model" type="text"
                                :state="validateState('instructions')"></b-form-input>
                            <b-form-invalid-feedback v-if="!$v.form.instructions.required">
                                instructions is required
                            </b-form-invalid-feedback>
                            <!-- <b-form-invalid-feedback v-if="!$v.form.instructions.alphaNum">
                                instructions alphaNum
                            </b-form-invalid-feedback> -->
                        </b-form-group>


                        <b-form-group id="input-group-numOfServings" label-cols-sm="3" label="Num of Servings:"
                            label-for="numOfServings">
                            <b-form-input id="numOfServings" v-model="$v.form.numOfServings.$model" type="text"
                                :state="validateState('numOfServings')"></b-form-input>
                            <b-form-invalid-feedback v-if="!$v.form.numOfServings.required">
                                numOfServings is required
                            </b-form-invalid-feedback>
                            <b-form-invalid-feedback v-if="!$v.form.numOfServings.numeric">
                                numOfServings should be numeric
                            </b-form-invalid-feedback>
                        </b-form-group>
                    </b-form>
                    <b-alert class="mt-2" v-if="form.submitError" variant="warning" dismissible show>
                        Create new recipe failed: {{ form.submitError }}
                    </b-alert>
                </div>
            </b-modal>
        </div>
    </div>
</template>
<script>
import {
    required,
    minLength,
    alpha,
    numeric,
    url
} from "vuelidate/lib/validators";

export default {
    name: "createRecipe",
    data() {
        return {
            form: {
                title: "",
                readyInMinutes: "",
                imageUrl: "",
                popularity: "",
                vegan: 0,
                vegetarian: 0,
                glutenFree: 0,
                ingredients: "",
                instructions: "",
                numOfServings: ""
            },
            errors: [],
            validated: false,
            modalShow : false,
        };
    },
    computed:{
    glutenFreeRes: {get(){
      return this.form.glutenFree
    },
    set(value){
      if(this.glutenFree!==0)
      {
        this.form.glutenFree = value
      }
    },
  },
  vegetarianRes: {get(){
      return this.form.vegetarian
    },
    set(value){
      if(this.vegetarian!==0)
      {
        this.form.vegetarian = value
      }
    },
  },
  veganRes: {get(){
      return this.form.vegan
    },
    set(value){
      if(this.vegan!==0)
      {
        this.form.vegan = value
      }
    },
  },
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
            },
            instructions: {
                required,
            },
            numOfServings: {
                required,
                numeric
            }
        }
    },
    mounted() {
    },
    methods: {
        validateState(param) {
            const { $dirty, $error } = this.$v.form[param];
            return $dirty ? !$error : null;
        },
        async CreateRecipe() {
            try {
                const response = await this.axios.post(
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
                    },
                );
                this.$router.push("/myRecipes");

                // console.log(response);
            } catch (err) {
                console.log(err.response);
                this.form.submitError = err.response.data.message;
            }
        },
        onCreate() {
            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }
            this.CreateRecipe();
            this.$emit('close-modal');
                  },
        onReset() {
            this.form = {
                title: "",
                readyInMinutes: "",
                imageUrl: "",
                popularity: "",
                vegan: 0,
                vegetarian: 0,
                glutenFree: 0,
                ingredients: "",
                instructions: "",
                numOfServings: ""
            };
            this.$nextTick(() => {
                this.$v.$reset();
            });
        },
    }
};
</script>