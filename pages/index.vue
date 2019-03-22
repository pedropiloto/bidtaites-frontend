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
  data: function() {
    return {
      auctions: [
        {
          id: "1",
          title: "Auction 1",
          price: 20,
          image_url:
            "https://pngimage.net/wp-content/uploads/2018/05/default-image-png-2.png",
          limit_date: new Date("Mar 24, 2019 15:37:25").getTime()
        },
        {
          id: "2",
          title: "Auction 2",
          price: 20,
          image_url:
            "https://pngimage.net/wp-content/uploads/2018/05/default-image-png-2.png",
          limit_date: new Date("Apr 26, 2019 15:37:25").getTime()
        }
      ]
    };
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
