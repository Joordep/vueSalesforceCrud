<template>
  <div id="app">              
      <md-table>
        <md-table-row>
          <md-table-head>Name</md-table-head>
          <md-table-head>Description</md-table-head>
          <md-table-head>Price</md-table-head>
          <md-table-head>Quantity</md-table-head>                  
        </md-table-row>
        
      <md-table-row>
        <md-table-cell>{{inventory.Name}}</md-table-cell>
        <md-table-cell>{{inventory.description__c}}</md-table-cell>
        <md-table-cell>{{inventory.price__c}}</md-table-cell>
        <md-table-cell>{{inventory.Quantity__c}}</md-table-cell>
        <md-button class="md-icon-button md-raised md-primary">
          <md-icon>save</md-icon>
        </md-button>          
      </md-table-row>
    
    </md-table>    
  </div>
</template>    

<script>

import axios from 'axios' 
import Vue from 'vue'
import VueMaterial from 'vue-material'
import 'vue-material/dist/vue-material.min.css'

Vue.use(VueMaterial)

export default {
  name: 'app',
  data(){
    return {
      inventory: []
    }
  },
  mounted: function(){
      axios({method: 'GET', url: 'http://localhost:8080/inventory'})      
      .then(response => (              
        this.inventory = response.data)
      )
  
  }
}
</script>

<style>

</style>
