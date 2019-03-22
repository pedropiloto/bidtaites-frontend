<template>
  <div>
    <Navbar/>
    <div class="container">
      <div class="row auctionCreate">
        <div class="col-sm-12">
          <div class="form-group">
            <label for="exampleInputEmail1">Title</label>
            <input
              type="text"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Item Title"
              v-model="itemTitle"
            >
          </div>

          <div class="form-group">
            <label for="exampleInputEmail1">Description</label>
            <input
              type="text"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Item Description"
              v-model="itemDescription"
            >
          </div>

          <div class="form-group">
            <label for="exampleInputEmail1">Email</label>
            <input
              type="email"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Email"
              v-model="contactEmail"
            >
          </div>

          <div class="form-group">
            <label for="exampleInputEmail1">Contact Number</label>
            <input
              type="text"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Contact Number"
              v-model="contactNumber"
            >
          </div>
          <div class="form-group">
            <label for="exampleInputEmail1">Base Price</label>
            <input
              type="number"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Base Price"
              v-model="basePrice"
            >
          </div>
          <div class="form-group">
            <label for="exampleInputEmail1">Image</label>
            <input
              type="text"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Image"
              v-model="image"
            >
          </div>
          <div class="form-group">
            <label for="exampleInputEmail1">Contact Number</label>
            <input
              type="text"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Contact Number"
              v-model="contactNumber"
            >
          </div>
          <div class="form-group">
            <label for="exampleInputEmail1">End Date</label>
            <date-picker v-model="date" :config="options"></date-picker>
          </div>
          <button class="btn btn-primary" v-on:click="register">Register Auction</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "~/components/Navbar.vue";
import axios from "axios";
import datePicker from "vue-bootstrap-datetimepicker";
import "pc-bootstrap4-datetimepicker/build/css/bootstrap-datetimepicker.css";

export default {
  components: {
    Navbar,
    datePicker
  },
  data: function() {
    return {
      itemTitle: "",
      itemDescription: "",
      basePrice: "",
      image: "",
      date: new Date(),
      options: {
        format: "DD/MM/YYYY",
        useCurrent: false
      }
    };
  },
  methods: {
    register: function() {
      console.log("dssdsds: " + this.date.getTime());
      axios.post("https://bidtaites.free.beeceptor.com/auctions").then(res => {
        console.log(res);
      });
    }
  }
};
</script>

<style>
.auctionCreate {
  margin-top: 4%;
}
</style>
