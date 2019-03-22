<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="navbar-brand">
      <router-link class="nav-link" to="/">
        <logo/>
      </router-link>
    </div>
    <button
      class="navbar-toggler"
      type="button"
      data-toggle="collapse"
      data-target="#navbarSupportedContent"
      aria-controls="navbarSupportedContent"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav mr-auto">
        <!-- <li class="nav-item">
          <router-link class="nav-link" to="/auction-create">Auction Creation</router-link>
        </li>-->
      </ul>
    </div>
  </nav>
</template>

<script>
import Logo from "~/components/Logo.vue";

export default {
  components: {
    Logo
  }
};
</script>

<style>
.navbar {
  margin-bottom: 30px;
}
</style>
