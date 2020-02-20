<template>
  <div id="app">
    <div>
      <md-list>
        <md-list-item>
          
          <md-field>
            <label>Name</label>
            <md-input v-model="inventoryItem.Name" ></md-input>            
          </md-field>
          
          <md-field>
            <label>Description</label>
            <md-input v-model="inventoryItem.description__c"></md-input>            
          </md-field>
          
          <md-field>
            <label>Price</label >
            <md-input v-model="inventoryItem.price__c" style="text-align: center" type="number"></md-input>
          </md-field>

          <md-field >
            <label>Quantity</label>
            <md-input v-model="inventoryItem.Quantity__c" style="text-align: center" type="number"></md-input>
          </md-field>

          <md-button
            class="md-icon-button md-raised md-primary"
            @click="createNewRecord(inventoryItem)"
          >
            <md-icon>save</md-icon>
          </md-button>
        </md-list-item>
      </md-list>
    </div>
   <hr>
    <md-table v-model="inventory" md-sort="name" md-sort-order="asc">
      <md-table-row>
        <md-table-head>Name</md-table-head>
        <md-table-head>Description</md-table-head>
        <md-table-head>Price</md-table-head>
        <md-table-head>Quantity</md-table-head>
      </md-table-row>

      <md-table-row slot="md-table-row" slot-scope="{ item }">
        <md-table-cell md-label="Name" md-sort-by="Name">{{item.Name}}</md-table-cell>
        <md-table-cell md-label="Description" md-sort-by="description__c">{{item.description__c}}</md-table-cell>
        <md-table-cell md-label="Price" md-sort-by="price__c">{{item.price__c}}</md-table-cell>
        <md-table-cell md-label="Quantity" md-sort-by="Quantity__c">{{item.Quantity__c}}</md-table-cell>
        <md-table-cell md-label="Edit">  
          <md-button class="md-icon-button md-accent" @click="editRecord(item)">
            <md-icon>edit</md-icon>
          </md-button>
        </md-table-cell> 
        <md-table-cell md-label="Delete">           
          <md-button class="md-icon-button md-accent" @click="deleteItem(item.Id)">
            <md-icon>delete</md-icon>
          </md-button>
        </md-table-cell> 
      </md-table-row>
    </md-table>
  </div>
</template>    

<script>
import axios from "axios";
import Vue from "vue";
import VueMaterial from "vue-material";
import "vue-material/dist/vue-material.min.css";

Vue.use(VueMaterial);

export default {
  name: "app",
  data() {
    return {
      inventory: [],

      inventoryItem: {        
        Name: "",
        price__c: "",
        description__c: "".
        Quantity__c
      }
    };
  },
  mounted: function() {
    this.list();
  },
  methods: {
    list() {
      axios({ method: "GET", url: "http://localhost:8080/getAll" }).then(
        response => (this.inventory = response.data.records)
      );
    },
    deleteItem(id) {
      if (confirm("Are you sure ?")) {
        axios({
          method: "DELETE",
          url: "http://localhost:8080/deleteById",
          params: { id: id }
        }).then(response => (console.log(response), this.list()));
      }
    },
    createNewRecord() {
      if(!this.inventoryItem.Id){
        axios
          .post("http://localhost:8080/createNew", this.inventoryItem)
          .then(
            response => (
              console.log(response),
              this.cleanFields(),
              this.list(),
              alert("Saved")
            )
          );
      }else{
        this.updateRecord()
      }
    },
    cleanFields() {
      this.inventoryItem = {};
    },
    editRecord(item){      
      this.inventoryItem.Id = item.Id
      this.inventoryItem.Name = item.Name
      this.inventoryItem.price__c = item.price__c
      this.inventoryItem.description__c = item.description__c
      this.inventoryItem.Quantity__c = item.Quantity__c
    },
    updateRecord(){
      axios
        .patch("http://localhost:8080/updateItem", this.inventoryItem)
        .then(
          response => (
            console.log(response),
            this.cleanFields(),
            this.list(),
            alert("Saved")
          )
        );
    }
  }
};
</script>

<style>
</style>
