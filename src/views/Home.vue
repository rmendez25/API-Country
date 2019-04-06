<template>
  <div>
    <v-container  text-md-center>
      <v-layout row wrap>
        <v-flex xs12 sm6 offset-sm3>
          <h1 class="display-1 mb-2">World's Countries</h1>
          <v-select
          :items="select"
          v-model="country"
          label="Select a Country"
          outline
        ></v-select>

        <h2 class="headline">Capital</h2>  
        <v-card dark color="blue-grey darken-3 mb-2">
            
            <v-card-text class="headline">
               {{capital}}
            </v-card-text>
        </v-card>
        <h1 class="display-1 mb-2">Countries Flags</h1>

        <v-card>
          <v-img v-bind:src="flag">
          </v-img>
        </v-card>

        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
  import axios from "axios";
  export default {
   data(){
     return{
       select:[],
       capital:null,
       country:null,
       flag:null
     }
   },
  methods:{
     async getCountryName(){
       try {
         let response = await axios.get(`https://restcountries.eu/rest/v2/all`)
        this.countryNames(response.data)
       } catch (error) {
         console.log(error)
       }
     },

      countryNames(response){
        response.forEach(element => {
          this.select.push(element.name)
        });
      },

      async getCountryInfo(){
        try {
          let response = await axios.get(`https://restcountries.eu/rest/v2/name/${this.country}?fullText=true`)
          this.getCapital(response.data)
        } catch (error) {
           console.log(error) 
        }
      },
      getCapital(response){
        response.forEach(element => {
          this.capital = element.capital
          this.flag = element.flag
        });
      },

   },
         
  watch:{
      country: function(newVal, oldVal){
        if(!newVal){
          return;
        }else{
          this.getCountryInfo()
        }
      }
    },
      
   

  created(){
     this.getCountryName()
   }
  }
</script>
