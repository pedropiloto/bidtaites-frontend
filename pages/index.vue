<template>
  <div>
    <Navbar/>
    <section class="container">
      <div>
        <div class="row">
          <ul class="thumbnails" v-for="item in auctions" :key="item.id">
            <AuctionItem v-bind:item="item"/>
          </ul>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import Logo from "~/components/Logo.vue";
import Navbar from "~/components/Navbar.vue";
import AuctionItem from "~/components/AuctionItem";
import { getTimeLeft } from "~/components/AuctionItem";
import axios from "axios";
import { API_URL, EXPIRED } from "~/config";

export default {
  components: {
    Logo,
    AuctionItem,
    Navbar
  },
  head() {
    return {
      title: "Bidtaites"
    };
  },
  created() {
    this.getAuctions();
    this.startTime();
  },
  data() {
    return {
      auctions: []
    };
  },
  methods: {
    getAuctions() {
      var url = API_URL + "/api/auctions";
      axios
        .get(url)
        .then(res => {
          this.auctions = res.data
            .sort((a, b) => {
              var tA = a.end_at;
              var tB = b.end_at;
              var timeLeftA = getTimeLeft(tA * 1000);
              var result;
              if (timeLeftA === EXPIRED) return 1;
              if (tA > tB) {
                return 1;
              } else if (tB > tA && tB) {
                return -1;
              } else {
                return 0;
              }
            })
            .reverse();
        })
        .catch(exception => {
          console.log(exception);
        });
    },
    startTime: function() {
      setInterval(() => {
        this.getAuctions();
      }, 6000);
    }
  }
};
</script>

<style>
.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
