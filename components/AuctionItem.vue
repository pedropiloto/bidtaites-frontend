<template>
  <div>
    <li class="span4 auctionCard">
      <div class="thumbnail">
        <div class="crop">
          <img v-bind:src="item.photo" alt="ALT NAME">
        </div>
        <div class="caption">
          <h3 class="auctionCard-title">{{item.title}}</h3>
          <div class="d-flex justify-content-between auctionItemDetails">
            <div>
              <small>Base</small>
              <span class="badge badge-pill badge-light">{{item.price}} €</span>
            </div>
            <div>
              <span class="badge badge-pill badge-light">{{endDate}}</span>
            </div>
          </div>
          <div>
            <small>Top Bid</small>
            <span class="badge badge-pill badge-light">{{maxBidAmount}} €</span>
          </div>
          <p align="center">
            <button
              id="show-modal"
              @click="showModal=true"
              class="btn btn-primary btn-block buttonBid"
              :disabled="!!expired"
            >Bid</button>
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
                  <h5 class="modal-title">Bid</h5>
                  <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true" @click="showModal = false">&times;</span>
                  </button>
                </div>
                <div class="modal-body">
                  <div class="row">
                    <div class="col-sm-12">
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
                        <label for="exampleInputEmail1">Value</label>
                        <input
                          type="number"
                          class="form-control"
                          aria-describedby="emailHelp"
                          placeholder="KTC"
                          v-model="bidValue"
                          :min="item.price"
                          v-on:keyup="bidAmountChangeHandler"
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
                    :disabled="!canBid"
                  >Bid now!</button>
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
import { API_URL } from "~/config";

export default {
  props: ["item"],
  data: function() {
    return {
      showModal: false,
      endDate: getTimeLeft(this.item.end_at * 1000),
      bidEmail: "",
      maxBidAmount: 0,
      expired: this.endDate === "EXPIRED",
      canBid: false
    };
  },
  mounted: function() {
    this.startTime();
    this.value = this.item.value;
    this.getDetails(this.item.uuid);
  },
  methods: {
    registerBid: function() {
      var url = API_URL + "/api/auctions/" + this.item.uuid + "/bids";
      var params = {
        auction_id: this.item.id,
        email: this.bidEmail,
        value: this.bidValue
      };
      axios.post(url, params).then(res => {
        window.location = res.data.redirect;
      });
    },
    getDetails(id) {
      var url = API_URL + "/api/auctions/" + this.item.uuid;
      axios
        .get(url)
        .then(res => {
          console.log(res.data.max_bid);
          this.maxBidAmount = res.data.max_bid ? res.data.max_bid : 0;
        })
        .catch(exception => {
          console.log(exception);
        });
    },
    startTime: function() {
      setInterval(() => {
        this.endDate = getTimeLeft(this.item.end_at * 1000);
        this.expired = this.endDate === "EXPIRED";
      }, 1000);
    },
    bidAmountChangeHandler() {
      this.canBid = this.bidValue > this.maxBidAmount;
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

.crop {
  height: 200px;
  width: 320px;
  overflow: hidden;
}

.crop img {
  max-width: 100%;
  width: 100%;
  top: 50%;
  left: 50%;
}

.auctionCard {
  background-color: #f8f9fa;
  margin-bottom: 4rem;
  border-radius: 2rem 2rem 1.5rem 1.5rem;

  box-shadow: 0 1px 1px 0 rgba(0, 0, 0, 0.14),
    0 2px 1px -1px rgba(0, 0, 0, 0.12), 0 1px 3px 0 rgba(0, 0, 0, 0.2);
}

.auctionCard-title {
  display: inline;
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.auctionCard .caption {
  padding: 2rem 1rem;
  max-width: 320px;
}

.auctionCard img {
  border-radius: 2rem 2rem 0 0;
}

.buttonBid {
  margin-top: 12%;
}
</style>
