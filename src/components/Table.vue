<template>
  <div id="app">
    
    <div class="col-6">

      <label for="search">Search:</label>
      <input type="text" id="search" v-model='search' class="w-100">

      <hr>

      <div v-if="search">
        <p v-for='(element, index) in showCars()'
          :key="element.id" 
          :style='{"background":element.color}'
          >
          {{index + 1}}.
          {{element.name}}: 
          {{element.price}}, 
          {{element.model}}, 
          {{element.transmission}},
          {{element.brakes}},
          {{element.color}},
          {{element.date}},
          {{element.description}}
        </p>
      </div>

      
      <table class="table table-hover text-center table-bordered">
        
        <thead class="thead-light">
          <tr>
            <th>#</th>
            <th>Name</th>
            <th>Price</th>
            <th>Model</th>
            <th>Transm</th>
            <th>Brakes</th>
            <th>Color</th>
            <th>Date_of_purchase</th>
            <th>Description</th>
            <th></th>
          </tr>
        </thead>


        <tr v-for="item in cars" :key="item.id">
          <td class="align-middle">{{item.id}}</td>
          <td class="align-middle">{{item.name}}</td>
          <td class="align-middle">{{item.price}}</td>
          <td class="align-middle">{{item.model}}</td>
          <td class="align-middle">{{item.transmission}}</td>
          <td class="align-middle">{{item.brakes.toString()}}</td>
          <td class="align-middle">{{item.color}}</td>
          <td class="align-middle">{{item.date}}</td>
          <td class="align-middle">{{item.description}}</td>
          <td>
            <div class="btn-group" role="group" aria-label="group">
              <button class="btn-sm btn-warning mr-1" @click='editCar(item.id)'>Edit</button>
              <button class="btn-sm btn-danger" @click="deleteCar(item.id)">Delete</button>
            </div>
          </td>
        </tr>
      </table>


      <router-link class="btn btn-primary" :to='"/add"'>Add new car</router-link>
    </div>
  </div>

</template>


<script>

import axios from 'axios'

export default {
  name: "App",
  
  data() {
    return {
      url: "http://localhost:3000/cars",
      cars: [],
      search: ''
    };
  },

  methods: {

    getCars() {
      // Creating a request to obtain elements
      axios
        .get(this.url)
        .then(data => data.data)
        .then(val => (this.cars = val));
    },


    // Removing the element from the table
    deleteCar(id) {
      let ask = confirm("Вы уверены?");
      if (ask) {
        axios
          .delete(this.url + "/" + id)
          .then(data => data.data)
          .then(() => this.getCars());
      }
    },


    // Redirection to the editing route
    editCar(id) {
      this.$router.push('/edit/'+id);
    },


    showCars() {
      return this.cars.filter(carItem => 
        carItem.name.toLowerCase().indexOf(this.search.toLowerCase()) !== -1 ||
        carItem.price.toString().toLowerCase().indexOf(this.search.toLowerCase()) !== -1 ||
        carItem.model.toLowerCase().indexOf(this.search.toLowerCase()) !== -1 ||
        carItem.transmission.toLowerCase().indexOf(this.search.toLowerCase()) !== -1 ||
        carItem.brakes.toString().toLowerCase().indexOf(this.search.toLowerCase()) !== -1 ||
        carItem.color.toLowerCase().indexOf(this.search.toLowerCase()) !== -1 ||
        carItem.date.toLowerCase().indexOf(this.search.toLowerCase()) !== -1 ||
        carItem.description.toLowerCase().indexOf(this.search.toLowerCase()) !== -1
        );
    }
  },


  // Initializing the car list when installing components
  created() {
    this.getCars();
  }
};
</script>


<style>
  p {
    width:w-100;
    text-align: center;
    color:white;
  }
</style>