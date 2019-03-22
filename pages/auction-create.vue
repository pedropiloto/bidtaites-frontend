<template>
  <div>
    <Navbar/>
    <div class="container">
      <div class="alert alert-success alert-dismissible fade show" role="alert" v-if="success">
        <strong>Success</strong> Created with success
        <button
          type="button"
          class="close"
          data-dismiss="alert"
          aria-label="Close"
        >
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="alert alert-danger alert-dismissible fade show" role="alert" v-if="fail">
        <strong>Fail</strong> Not Created with success
        <button
          type="button"
          class="close"
          data-dismiss="alert"
          aria-label="Close"
        >
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="row auctionCreate">
        <div class="col-sm-12">
          <div class="form-group">
            <label for="exampleInputEmail1">Title</label>
            <input
              type="text"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Item Title"
              v-model="title"
            >
          </div>

          <div class="form-group">
            <label for="exampleInputEmail1">Description</label>
            <input
              type="text"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Item Description"
              v-model="description"
            >
          </div>

          <div class="form-group">
            <label for="exampleInputEmail1">Photo</label>
            <input
              type="text"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Photo"
              v-model="photo"
            >
          </div>

          <div class="form-group">
            <label for="exampleInputEmail1">Base Price</label>
            <input
              type="number"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Base Price"
              v-model="price"
            >
          </div>

          <div class="form-group">
            <label for="exampleInputEmail1">Email</label>
            <input
              type="email"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Email"
              v-model="email"
            >
          </div>

          <div class="form-group">
            <label for="exampleInputEmail1">Phone Number</label>
            <input
              type="text"
              class="form-control"
              aria-describedby="emailHelp"
              placeholder="Phone Number"
              v-model="phone_number"
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
import { API_URL } from "~/config";
import Snotify from "vue-snotify"; // 1. Import Snotify
import Noty from "noty";

export default {
  components: {
    Navbar,
    datePicker,
    Snotify
  },
  data: function() {
    return {
      title: "",
      description: "",
      photo: "",
      price: "",
      email: "",
      phone_number: "",
      date: new Date(),
      options: {
        format: "MM/DD/YYYY",
        useCurrent: false
      },
      success: false,
      fail: false
    };
  },
  methods: {
    register: function() {
      var url = API_URL + "/api/auctions";
      var params = {
        title: this.title,
        description: this.description,
        photo: this.photo,
        price: this.price,
        email: this.email,
        phone_number: this.phone_number,
        end_at: Math.round(new Date(this.date).getTime() / 1000)
      };
      axios
        .post(url, params)
        .then(res => {
          this.success = true;
          this.fail = false;

          //clean fields
          this.title = "";
          this.description = "";
          this.photo = "";
          this.price = "";
          this.email = "";
          this.phone_number = "";
        })
        .catch(exception => {
          this.success = false;
          this.fail = true;
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
