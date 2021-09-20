<template>
  <div id="app">

    <div class="row">
      <div class = "col-sm-3">
    <globalmenu/>
      </div>
    <div class = "col-sm-9">

      <table style="margin: 30px;">
        <tr>
          <th> Recipe Management </th>
        </tr>
      </table>
  
      <table class="table" style="font-size: 11px;">
        <thead>
          <th scope="col"> Id </th>
          <th scope="col"> Recipe Title </th>
          <th scope="col"> Is the Recipe Vegeterian ? </th>
          <th scope="col"> Suitable For </th>
          <th scope="col"> Ingredients </th>
          <th scope="col"> Cooking Instructions </th>
          <th scope="col"> Creation Date </th>
          <th scope="col"> Update Date </th>
          <th></th>
        </thead>
        <tbody>
    <tr v-for="item in recipies" :key="item">
    <td>{{ item.id }}</td>
    <td>{{ item.recipeTitle }}</td>
    <td>{{ item.isVegeterian }}</td>
    <td>{{ item.suitableForPeople }}</td>
    <td>{{ item.ingredients }}</td>
    <td>{{ item.cookingInstructions }}</td>
    <td>{{ item.createdDate }}</td>
    <td>{{ item.updatedDate }}</td>
    <td> 
      <div class="btn btn-group">
        <button class="btn btn-sm btn-warning" v-on:click="edit(item)">Manage</button> 
        <button class="btn btn-sm btn-danger" v-on:click="deleteItem(item)">Delete</button>
      </div>
    </td>
    </tr>
    </tbody>
      </table>
    </div>
      
  </div>
</div>
</template>

<script>
import globalmenu from './Menu.vue';
import Constants from "./Constants.js";

export default {
  name: 'App',
  components: {
    globalmenu
  },
      data: function () {
        return {
            recipies: [],
        }
    },
    
    methods: {
        async getDefaultFilter() {

            const headers = {

            'Authorization': 'Bearer '+localStorage.getItem('user-token'),
            "Access-Control-Allow-Origin": "*"
          }

           try {
             const response = await this.$http.post(
               Constants.URL+'/api/recipe/all',{},{headers:headers});
               this.recipies=response.data;

        } catch (error) {
        console.log(error);
      
      }
        },

        async edit( item ) {

          this.$router.push(this.$router.push({name: 'addrecipe', params: { recipeObject: item }}));

        },

        async deleteItem( item ) {
                      const headers = {
            'Content-Type': 'application/json',
            'Authorization': 'Bearer '+localStorage.getItem('user-token')
    }

 
     
           try {
               
                this.errorMessage ="";
        const response = await this.$http.post(
          Constants.URL+"/api/recipe/delete",{id:item.id},{headers:headers});
             
                  alert(response.data['message']);
            this.$router.go();
            
           

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
        }
    },
    beforeMount() {
        this.getDefaultFilter();
    }
}

</script>


