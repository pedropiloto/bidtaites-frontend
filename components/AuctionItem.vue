<template>
  <div>
    <li class="span4">
      <div class="thumbnail">
        <img v-bind:src="item.image_url" style="width:320px;height:200px" alt="ALT NAME">
        <div class="caption">
          <h3>{{item.title}}</h3>
          <div class="d-flex justify-content-between auctionItemDetails">
            <div>
              <span class="badge badge-pill badge-light">90</span>
            </div>
            <div>
              <span class="badge badge-pill badge-light">{{endDate}}</span>
            </div>
          </div>
          <p align="center">
            <button id="show-modal" @click="showModal=true" class="btn btn-primary btn-block">Bid</button>
          </p>
        </div>
      </div>
    </li>

    <div v-if="showModal">
      <transition name="modal">
        <div class="modal-mask">
          <div class="modal-wrapper">
            <div class="modal-dialog" role="document">
              <div class="modal-content">
                <div class="modal-header">
                  <h5 class="modal-title">Modal title</h5>
                  <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true" @click="showModal = false">&times;</span>
                  </button>
                </div>
                <div class="modal-body">
                  <div class="row">
                    <div class="col-sm-12">
                      <div class="form-group">
                        <label for="exampleInputEmail1">Contact Number</label>
                        <input
                          type="text"
                          class="form-control"
                          aria-describedby="emailHelp"
                          placeholder="Contact Number"
                          v-model="bidContactNumber"
                        >
                      </div>
                      <div class="form-group">
                        <label for="exampleInputEmail1">Email</label>
                        <input
                          type="text"
                          class="form-control"
                          aria-describedby="emailHelp"
                          placeholder="Email"
                          v-model="bidEmail"
                        >
                      </div>
                      <div class="form-group">
                        <label for="exampleInputEmail1">Bid Amount</label>
                        <input
                          type="number"
                          class="form-control"
                          aria-describedby="emailHelp"
                          placeholder="Bid"
                          v-model="bidAmount"
                        >
                      </div>
                    </div>
                  </div>
                </div>
                <div class="modal-footer">
                  <button type="button" class="btn btn-secondary" @click="showModal = false">Close</button>
                  <button
                    type="button"
                    class="btn btn-primary"
                    v-on:click="registerBid"
                  >Save changes</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  props: ["item"],
  data: function() {
    return {
      showModal: false,
      endDate: getTimeLeft(this.item.limit_date),
      bidContactNumber: "",
      bidEmail: "",
      bidAmount: ""
    };
  },
  created: function() {
    console.log("created");
    this.startTime();
  },
  methods: {
    registerBid: function() {
      console.log("dssdsds");
      axios.post("https://bidtaites.free.beeceptor.com/auctions").then(res => {
        console.log("item: " + this.item.id + "res: " + res);
      });
    },
    startTime: function() {
      setInterval(() => {
        var limitDate = this.item.limit_date;
        this.endDate = getTimeLeft(this.item.limit_date);
      }, 1000);
    }
  }
};

var getTimeLeft = limitDate => {
  // Get todays date and time
  var now = new Date().getTime();

  // Find the distance between now and the count down date
  var distance = limitDate - now;

  // Time calculations for days, hours, minutes and seconds
  var days = Math.floor(distance / (1000 * 60 * 60 * 24));
  var hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
  var minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
  var seconds = Math.floor((distance % (1000 * 60)) / 1000);

  return distance >= 0
    ? days + "d " + hours + "h " + minutes + "m " + seconds + "s "
    : "EXPIRED";
};
</script>

<style>
ul {
  list-style-type: none;
}

.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.auctionItemDetails {
  margin-top: 1%;
  margin-bottom: 1%;
}
</style>