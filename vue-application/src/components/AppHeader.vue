<template>
  <header>
    <div class="container header__items-wrap">
      <h2>CarsJS</h2>

      <div class="header__right-side">
        <button class="header__create-car-btn" type="button" @click="openCreateCarModal">Add car</button>
        <form @submit.prevent="search">
          <input type="search" placeholder="Type search query..." v-model="searchQuery">
        </form>

        <router-link to="/auth">
          <button class="primary" type="button" @click="logout">Logout</button>
        </router-link>
      </div>
    </div>
  </header>
</template>


<script>
import { mapGetters, mapActions } from "vuex";

export default {
  data() {
    return {
      searchQuery: ""
    };
  },
  computed: {
    /**
     * Get state of options for GET request
     */
    ...mapGetters(["QUERY_DATA"])
  },
  methods: {
    /**
     * Get acions from store
     */
    ...mapActions(["GET_CARS", "CHANGE_CARS_MODAL_OPTIONS", "LOGOUT"]),

    /**
     * Get cars list based on input search query
     */
    search() {
      this.GET_CARS({
        page: 1,
        keyword: this.searchQuery,
        orderBy: this.QUERY_DATA.orderBy,
        sortOrder: this.QUERY_DATA.sortOrder
      });
    },

    /**
     * Method for open modal window
     */
    openCreateCarModal() {
      this.CHANGE_CARS_MODAL_OPTIONS({ opened: true });
    },

    /**
     * Method for logout and redirect to auth page
     */
    logout() {
      this.$router.push(this.$route.query.redirect || "/auth");

      this.LOGOUT();
    }
  }
};
</script>

<style scoped>
header {
  background-color: rgb(50, 50, 50);
  box-shadow: rgba(0, 0, 0, 0.13) 0px 2px 6px 0px;
  margin-bottom: 5px;
  width: 100vw;
  height: 50px;
}

h2 {
  color: #fff;
  font-weight: bold;
  font-size: 18px !important;
  margin: 0 !important;
}

.header__items-wrap {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0 50px;
  height: 100%;
}

.header__right-side {
  display: flex;
}

.header__create-car-btn {
  background-color: #28a745;
  color: #fff;
  border: 1px solid #28a745;
}

.header__create-car-btn:hover {
  background-color: #2cb64c;
}
</style>
