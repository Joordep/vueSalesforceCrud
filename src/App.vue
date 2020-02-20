<template>
  <div id="app">              
        <form >

          <label>Nome</label>
          <input type="text"  v-model="inventoryItem.name">
          <label>Price</label>
          <input type="number"  v-model="inventoryItem.price__c">          
          
          <md-button class="md-icon-button md-raised md-primary" @click="createNewRecord(inventoryItem)">
            <md-icon>Salvar</md-icon>
          </md-button> 
          

      </form>

      <md-table v-model="inventory" md-sort="name" md-sort-order="asc">
        <md-table-row >
          <md-table-head>Name</md-table-head>
          <md-table-head>Description</md-table-head>
          <md-table-head>Price</md-table-head>
          <md-table-head>Quantity</md-table-head>                  
        </md-table-row>
        
      <md-table-row slot="md-table-row" slot-scope="{ item }">
        <md-table-cell md-label="Name" md-sort-by="Name" >{{item.Name}}</md-table-cell>
        <md-table-cell md-label="Description" md-sort-by="description__c" >{{item.description__c}}</md-table-cell>
        <md-table-cell md-label="Price" md-sort-by="price__c" >{{item.price__c}}</md-table-cell>
        <md-table-cell md-label="Quantity" md-sort-by="Quantity__c" >{{item.Quantity__c}}</md-table-cell>
        <md-button class="md-icon-button md-raised md-primary" @click="deleteItem(item.Id)">
          <md-icon>delete</md-icon>
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
      inventory: [],
      
      inventoryItem:{
        name: '',
        price__c:''
      }
    }
      
  },
  mounted: function(){
    this.list()      
  
  },
  methods: {
    list(){
      axios({method: 'GET', url: 'http://localhost:8080/getAll'})      
      .then(response => (              
        this.inventory = response.data.records)
      )
    },
    deleteItem(id){
      if(confirm('Are you sure ?')) { 
        axios({method: 'DELETE', url: 'http://localhost:8080/deleteById', params: {id: id}})      
          .then(response => (              
            console.log(response),
            this.list())
            
        )}      
    },
    createNewRecord(){
      axios.post('http://localhost:8080/createNew', this.inventoryItem)
       .then(response => ( 
         console.log(response),   
          this.cleanFields(),
          this.list(),     
           alert('Saved'))  
          
       )  
    },
    cleanFields(){
         this.inventoryItem = {}
    }    

  }
}
</script>

<style>

</style>
