<template>
  <div>
    <Loader v-if="loading" />
    <div v-else class="app-main-layout">
      <Navbar @handleSidebar="isVisibleSidebar = !isVisibleSidebar" />
      <Sidebar v-model="isVisibleSidebar" />

      <main class="app-content" :class="{ full: !isVisibleSidebar }">
        <div class="app-page">
          <router-view />
        </div>
      </main>

      <div class="fixed-action-btn">
        <router-link class="btn-floating btn-large blue" to="/record">
          <i class="large material-icons">add</i>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "../components/app/Navbar";
import Sidebar from "../components/app/Sidebar";

export default {
  name: "MainLayout",
  data() {
    return {
      isVisibleSidebar: true,
      loading: true,
    };
  },
  components: {
    Navbar,
    Sidebar,
  },
  async mounted() {
    if (!Object.keys(this.$store.getters.info).length) {
      await this.$store.dispatch("getInfo");
    }

    this.loading = false;
  },
};
</script>

<style>
</style>