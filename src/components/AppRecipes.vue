<template>
  <div class="jumbotron vertical-center">
      <div class = "container">
          <div class = "row">
              <div class = "col-sm-12">
                  <h1>Recipes</h1>
                  <hr />
                  <!-- Allert Message -->
        <b-alert v-if="showMessage" variant="success" show>{{
          message
        }}</b-alert>
        <!-- b-alert v-if="error" variant="danger" show>{{ error }}</b-alert-->

        <button
          type="button"
          class="btn btn-success btn-sm"
          v-b-modal.recipe-modal
        >
          Create Recipe
        </button>
                  <br /> <br />
                  <table class = "table table-hover">
                      <thead>
                          <tr>
                              <th scope = "col"> Recipe Name </th>
                              <th scope = "col"> Recipe Ingredients </th>
                              <th scope = "col"> Recipe Instructions </th>
                              <th scope = "col"> Recipe Favorite </th>
                              <th scope = "col"> Recipe Rating </th>
                          </tr>
                      </thead>
                      <tbody>
                          <tr v-for= "recipe in recipes" :key = "recipe.id">
                              <td> {{ recipe.name }} </td>
                              <td> {{ recipe.ingredients }} </td>
                              <td> {{ recipe.instructions }} </td>
                              <td>

                                  <b-icon 
                                  v-if="recipe.favorite == true" icon="lightning-charge-fill">
                                  </b-icon>

                                  <b-icon v-if="(recipe.favorite == false)" icon="lightning-charge">
                                  </b-icon>

                              </td>
                                <td> 
                                    <div>
                                        <b-form-rating v-model= "recipe.rating" readonly variant="warning" class="mb-2"></b-form-rating>
                                        <p class="mt-2"></p>
                                    </div> 
                                 </td>
                              
                               <td> 
                                  <div class="btn-group" role="group">
                                      <button
                                      type="button"
                                      class="btn btn-info btn-sm"
                                      v-b-modal.edit-recipe-modal
                                      @click="editRecipe(recipe)"
                                      >
                                      Edit
                                      </button>
                                      <button
                                      type="button"
                                      class="btn btn-danger btn-sm"
                                      @click="deleteRecipe(recipe)"
                                      >
                                      Delete
                                      </button>
                                  </div>
                              </td>
                          </tr>
                      </tbody>
                  </table>
                  <footer class = "text-center">
                      <p> &copy; 2022- 2023 </p>
                  </footer>
              
              </div>
          </div>
          <!-- Beginning of Modal for Create Recipe-->
              <div>   
            <b-modal 
              ref = "addRecipeModal"
              id="recipe-modal"
              title="Add Recipe"
              hide-backdrop
              hide-footer
              >
              <b-form @submit="onSubmit" class="w-100">
              <b-form-group
                  id="form-name-group"
              label="Recipe Name:"
              label-for="form-name-input"
              description="ENTER NAME"
              >
              <b-form-input
              id="form-name-input"
              type="text"
              v-model="createRecipeForm.name"
              placeholder="Recipe Name"
              required
              >
              </b-form-input>
              </b-form-group>
              <b-form-group
              id="form-ingredients-group"
              label="Recipe Ingredients:"
              label-for="form-ingredients-input"
              description="ENTER INGREDIENTS"
              >
              <b-form-input
              id="form-ingredients-input"
              type="text"
              v-model="createRecipeForm.ingredients"
              placeholder="Recipe Ingredients"
              required
              ></b-form-input>
              </b-form-group>

              <b-form-group
              id="form-instructions-group"
              label="Recipe Instructions:"
              label-for="form-instructions-input"
              description="ENTER INSTRUCTIONS"
              >
              <b-form-input
              id="form-instructions-input"
              type="text"
              v-model="createRecipeForm.instructions"
              placeholder="Recipe Instructions"
              required
              ></b-form-input>
              </b-form-group>
              <b-form-group
              id="form-favorite-group"    
              label="Recipe Favorite:"
              label-for="form-favorite-input"
              description="FAVORITE RECIPE?"
              >
              <b-form-checkbox
              id="form-favorite-input"
              v-model="createRecipeForm.favorite"
              ></b-form-checkbox>
              </b-form-group>
              <b-form-group
              id="form-rating-group"
              label="Recipe Rating:"
              label-for="form-rating-input"
              description="Rate Recipe 1-5"
              >
              <b-form-rating
              id="form-rating-input"
              type="integer"
              v-model="createRecipeForm.rating"
              variant="danger"
              ></b-form-rating>
              </b-form-group>
              <b-button type="submit" variant="primary">Submit</b-button>
              </b-form>
          </b-modal>

          <!-- End of Modal for Create Recipe-->

          <!-- Beginning of Modal for Edit Recipe-->
     
                
          <b-modal
            ref = "editRecipeModal"
            id="edit-recipe-modal"
            title="Edit Recipe"
            hide-backdrop
            hide-footer
          >
          <b-form @submit="onSubmitUpdate" class="w-100">
          <b-form-group
          id="form-edit-name-group"
          label="Recipe Name:"
          label-for="form-edit-name-input"
          description="ENTER NAME"
          >
          <b-form-input
          id="form-edit-name-input"
          type="text"
          v-model="updateRecipeForm.name"
            >
          </b-form-input>
          </b-form-group>
          <b-form-group
          id="form-edit-ingredients-group"
          label="Recipe Ingredients:"
          label-for="form-edit-ingredients-input"
          description="ENTER INGREDIENTS"
          >
          <b-form-input
          id="form-edit-ingredients-input"
          type="text"
          v-model="updateRecipeForm.ingredients"
          
          >
          </b-form-input>
          </b-form-group>
          <b-form-group
          id="form-edit-instructions-group"
          label="Recipe Instructions:"
          label-for="form-edit-instructions-input"
          description="ENTER INSTRUCTIONS"
          >
          <b-form-input

          id="form-edit-instructions-input"
          type="text"
          v-model="updateRecipeForm.instructions"
          
          >
          </b-form-input>
          </b-form-group>

          <b-form-group

          id="form-edit-favorite-group"
          label="Recipe Favorite:"
          label-for="form-edit-favorite-input"
          description="FAVORITE RECIPE?"
          >
          <b-form-checkbox
          id="form-favorite-input"
          v-model="updateRecipeForm.favorite"
          switch
          ></b-form-checkbox>
          </b-form-group>
          <b-form-group
          id="form-edit-rating-group"
          label="Recipe Rating:"
          label-for="form-edit-rating-input"
          description="Rate Recipe 1-5"
          >
          <b-form-rating
          id="form-edit-rating-input"
          type="integer"
          v-model="updateRecipeForm.rating"
          
          ></b-form-rating>
          </b-form-group>
          <b-button type="submit" variant="outline-info">Update</b-button>
          </b-form>
          </b-modal>
          <!-- End of Modal for Edit Recipe-->


      </div>
  
    
  
  </div>
</div>
 
  

</template>

<script>


import axios from 'axios';
export default {
  name: 'AppRecipes',
  data(){
      return {
          recipes: [],
          createRecipeForm: {
                id: '',
                name: '',
                ingredients: '',
                instructions: '',
                favorite: false,
                rating: 0,
          },

          updateRecipeForm: {
                id: '',
                name: '',
                ingredients: '',
                instructions: '',
                favorite: false,
                rating: 0,
          },

          showMessage: false,
          message: '',

      };
  },

  methods: {


    /***************************************************
     * RESTful requests
     ***************************************************/
    
      //GET Recipes

      RESTgetRecipes() {
      const path = `${process.env.VUE_APP_ROOT_URL}/`;
      axios
          .get(path)
          .then((response) => {
              this.recipes = response.data.recipes;
          })
          .catch((error) => {
              console.log(error);
          });
      },

      //POST Recipes
      RESTcreateRecipe(payload){
          const path = `${process.env.VUE_APP_ROOT_URL}/`;
          axios
              .post(path,payload)
              .then((response) => {
                  this.RESTgetRecipes();
                  this.message = "Recipe Created";
                  // To actually show the message
                  this.showMessage = true;
                  // To hide the message after 3 seconds
                  setTimeout(() => {
                    this.showMessage = false;
                  }, 3000);
              })
              .catch((error) => {
                  console.log(error);
                  this.RESTgetRecipes()
              });
      },

      RESTupdateRecipe(payload, recipeId){
          const path = `${process.env.VUE_APP_ROOT_URL}/${recipeId}`;
          axios
              .put(path,payload)
              .then((response) => {
                  this.RESTgetRecipes();
                    this.message = "Recipe Updated"
                    this.showMessage = true;
                    setTimeout(() => {
                        this.showMessage = false;
                    }, 3000);
                    
              })
              .catch((error) => {
                  console.log(error);
                  this.RESTgetRecipes()
              });
      },


      RESTdeleteRecipe(recipeId){
          const path = `${process.env.VUE_APP_ROOT_URL}/${recipeId}`;
          axios
              .delete(path)
              .then((response) => {
                  this.RESTgetRecipes();
                  this.message = "Recipe Deleted"
                  this.showMessage = true;
                  set.TimeOut(() => {
                      this.showMessage = false;
                  }, 3000);

              })
              .catch((error) => {
                  console.error(error);
                  this.RESTgetRecipes()
              });
      },

      initForm(){
          this.createRecipeForm = {
              name: '',
              ingredients: '',
              instructions: '',
              favorite: false,
              rating: 0
          };
          this.updateRecipeForm = {
              name: '',
              ingredients: '',
              instructions: '',
              favorite: false,
              rating: 0
          };

      },
      onSubmit(e) {
          e.preventDefault(); 
          this.$refs.addRecipeModal.hide(); 
          const payload = {
              name: this.createRecipeForm.name,
              ingredients: this.createRecipeForm.ingredients,
              instructions: this.createRecipeForm.instructions,
              favorite: this.createRecipeForm.favorite,
              rating: this.createRecipeForm.rating
          };
          this.RESTcreateRecipe(payload);
          this.initForm();
      },
      onSubmitUpdate(e) {
          e.preventDefault(); 
          this.$refs.editRecipeModal.hide(); 
          const payload = {
              name: this.updateRecipeForm.name,
              ingredients: this.updateRecipeForm.ingredients,
              instructions: this.updateRecipeForm.instructions,
              favorite: this.updateRecipeForm.favorite,
              rating: this.updateRecipeForm.rating
          };
          this.RESTupdateRecipe(payload, this.updateRecipeForm.id);
          this.initForm();
      },

      deleteRecipe(recipe) {
          this.RESTdeleteRecipe(recipe.id);
      },

      editRecipe(recipe) {
          this.updateRecipeForm = recipe;
      },

      

  },

   /***************************************************
 * LIFECYClE HOOKS
 ***************************************************/

  created() {
      this.RESTgetRecipes();
  },
};

</script>
