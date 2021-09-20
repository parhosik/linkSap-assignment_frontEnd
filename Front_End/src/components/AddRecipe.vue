<template>

<div class="row">
      <div class = "col-sm-3">
    <globalmenu/>
      </div>
    <div class = "col-sm-9">

    <div class="vue-tempalte">
                 <div >
      <div class="vertical-center">
        <div class="inner-block">
                 <h5 class="errorMessage">{{errorMessage}}</h5>
        <form>
            <h3>Add Recipe</h3>

<div class="row">

  <div class="col-sm-4"> 
    <div class="form-group">
        <label>Recipe Title</label>
        <input  v-model="theRecipy.id"  type="hidden" />
        <input  v-model="theRecipy.recipeTitle"  type="text" class="form-control form-control-lg"/>
    </div>
  </div>

  <div class="col-sm-2"> 
    <div class="form-group">
        <label>Suitable For</label>
        <input  v-model="theRecipy.suitableForPeople" placeholder="no#" type="number" class="form-control form-control-lg" />
    </div>
  </div>

  <div class="col-sm-2">
    <div class="form-group">
                <label>Is Vegeterian ?</label>
                <select v-model="theRecipy.isVegeterian" type="select" class="form-control form-control-lg" >
                  <option value="Yes"> Yes </option>
                  <option value="No"> No </option>
                </select>
            </div>
  </div>

</div>

<div class="row">

  <div class="col-sm-12">
      <div class="form-group">
          <label>Cooking Instructions</label>
          <input  v-model="theRecipy.cookingInstructions" type="text" class="form-control form-control-lg" />
      </div>
  </div>

</div>


<div class="row">

  <div class="col-sm-5">
    <div class="form-group">
        <label>Ingredients</label>
        <input  v-model="theIngredient" type="text" class="form-control form-control-lg" />
    </div>
  </div>

  <div class="col-sm-3">
    <div style="margin-top: 48px; margin-left: -25px;">
        <button v-on:click= "addIngredient(theIngredient)" type="button" class="btn btn-dark btn-sm"> Add Ingredient </button>
    </div>
  </div>

</div>

<div class="row">

  <div class="col-sm-6">

    <ul id="example-1">
      <li v-for="item in listOfIngredients" :key="item.message">
        {{ item }}
      </li>
    </ul>

  </div>

</div>


            


            

            

            

            <button v-on:click= "addRecipe()" type="button" class="btn btn-dark btn-lg btn-block"> Record My Delicious Recipe </button>


        </form>
    </div></div></div></div></div></div>
</template>

<script>
import globalmenu from './Menu.vue';
import Constants from "./Constants.js";
    export default {
         theRecipy:{},
         listOfIngredients:[],
         errorMessage:String,
          components: {
    globalmenu
  },
        data() {
            return {
                   
                    theRecipy:this.$route.params.recipeObject,
                    listOfIngredients:[],
                   errorMessage:this.errorMessage
            }
        },
              methods: {
      async addRecipe () {
          const headers = {
  'Authorization': 'Bearer '+localStorage.getItem('user-token')
    }

 
     
           try {
                this.errorMessage ="";
        const response = await this.$http.post(
          Constants.URL+"/api/recipe/add",
          {
            recipeId:this.theRecipy.id,
            recipeTitle:this.theRecipy.recipeTitle,
            suitableForPeople:this.theRecipy.suitableForPeople,
            isVegeterian:this.theRecipy.isVegeterian,
            cookingInstructions:this.theRecipy.cookingInstructions,
            ingredients:this.listOfIngredients.join(",", "" ),
          },
          {headers:headers});
            response.data['status'];
              alert("Delicious! We recorded your recipe.")
            this.$router.push('home'); 
           

        } catch (error) {
            if(error.response.data['error_description']!=null)
            this.errorMessage = error.response.data['error_description'];
            else if(error.response.data['message']!=null)
            this.errorMessage = error.response.data['message'];
            else if(error.response.data['fieldErrors']!=null)
            this.errorMessage = error.response.data['fieldErrors'];

            else
            this.errorMessage = "Network Error"
      }
        
      },

      async addIngredient(theIngredient) {
        this.listOfIngredients.push( theIngredient );
        this.theIngredient = '';
      }
      
    }, 
    
    beforeMount() {

        if( this.$route.params.recipeObject != null ) {
          this.listOfIngredients = this.$route.params.recipeObject.ingredients.split(",");
          this.theRecipy = {
              id:this.$route.params.recipeObject.id,
              recipeTitle:this.$route.params.recipeObject.recipeTitle,
              suitableForPeople:this.$route.params.recipeObject.suitableForPeople,
              isVegeterian:this.$route.params.recipeObject.isVegeterian,
              cookingInstructions:this.$route.params.recipeObject.cookingInstructions,
              ingredients:this.$route.params.recipeObject.ingredients
          }
        } else {
          this.theRecipy = {
              id:0,
              recipeTitle:'',
              suitableForPeople:'',
              isVegeterian:'',
              cookingInstructions:'',
              ingredients:''
          }
        }
    }

    }
</script>