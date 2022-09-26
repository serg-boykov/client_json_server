<template>
  <div id="app">
    <form class="col-4 offset-4">
      <legend v-if="adding">Cars adding</legend>
      <legend v-else>Car edit</legend>
      

      <div class="form-group">
        <label for="name">Car name</label>
        <input
          type="text"
          id="name"
          class="form-control"
          placeholder="enter car name"
          v-model="carName" 
        >
      </div>

      
      <div class="form-group">
        <label for="price">Car price</label>
        <input
          type="number"
          id="price"
          class="form-control"
          placeholder="enter car price"
          v-model="carPrice"
        >
      </div>
      

      <div class="form-group">
        <label for="model">Car model</label>
        <input
          type="text"
          id="model"
          class="form-control"
          placeholder="enter car model"
          v-model="carModel"
        >
      </div>


      <div class="form-group">
        <label class="mr-3">Transmission:</label>
        <input 
          type="radio" 
          name='tr' 
          value='auto'
          v-model="carTransm"> Automatic
        <input 
          type="radio" 
          name='tr' 
          value='manual' 
          class='ml-1'
          v-model="carTransm"> Manual
      </div>
      

      <div class="form-group">
        <label class="mr-3">Type of brakes:</label>
        <input 
          type="checkbox" 
          value='disc'
          v-model='carBrakes'> Disc
        <input 
          type="checkbox" 
          value='drum' 
          class='ml-1'
          v-model='carBrakes'> Drum
      </div>
      

      <div class="form-group">
        <label class="mr-3">Color</label>
        <select class='form-control' v-model="carColor">
          <option 
            :value='color' v-for='color in colors' :key='color'
          >{{color}}</option>
        </select>
      </div>
      

      <div class="form-group">
        <label for="date">Date of purchase</label>
        <input
          type="date"
          id="date"
          class="form-control"
          v-model="carDate"
        >
      </div>      
      

      <div class="form-group">
        <label for="description">Description</label>
        <textarea
          id="description" 
          class='form-control' 
          placeholder="enter car description"
          v-model="carDescription">
        </textarea>
      </div>
      

      <button v-if="adding" type="button" class="btn btn-success mr-1" @click="addCar">Add new car</button>
      <button v-if="!adding" type="button" class="btn btn-primary mr-1" @click="editCar(carId)">Edit car</button>
      <button class="btn btn-secondary" @click="goBack">Return</button>
    </form>
  </div>
</template>


<script>

import axios from 'axios'

export default {
  name: "app",
  
  data() {
    return {
      carId: 0,
      carName: "",
      carPrice: 0,
      carModel: "",
      carTransm: "",
      carBrakes: [],
      carColor: "",
      carDate: "",
      carDescription: "",
      url: "http://localhost:3000/cars",
      adding: true,
      colors: ["Red", "Magenta", "Orange", "Yellow", "Green", "Blue", "Purple"]
    };
  },


  methods: {
    
    addCar() {
      let newObj = {
        name: this.carName,
        price: this.carPrice,
        model: this.carModel,
        transmission: this.carTransm,
        brakes: this.carBrakes,
        color: this.carColor,
        date: this.carDate,
        description: this.carDescription
      };

      axios
        .post(this.url, newObj)
        .then(data => data.data)
        .then(() => {
          this.goBack();
        });
    },


    getCar(id) {
      axios
        .get(this.url + "/" + id)
        .then(data => data.data)
        .then(val => {
          this.carName = val.name;
          this.carPrice = val.price;
          this.carModel = val.model;
          this.carTransm = val.transmission;
          this.carBrakes = val.brakes;
          this.carColor = val.color;
          this.carDate = val.date;
          this.carDescription = val.description;
          this.carId = val.id;
        });
    },


    goBack() {
      this.$router
        .push("/home");
    },


    editCar(id) {
      let carObj = {
        name: this.carName,
        price: this.carPrice,
        model: this.carModel,
        transmission: this.carTransm,
        brakes: this.carBrakes,
        color: this.carColor,
        date: this.carDate,
        description: this.carDescription
      };

      axios
        .put(this.url + "/" + id, carObj)
        .then(val => val.data)
        .then(() => this.goBack());
    }
  },

  
  created() {
    let params = this.$route.params;
    if (params.id) {
      this.adding = false;
      this.getCar(params.id);
    }
  }
};
</script>


<style>

</style>