<template>
  <div id="App">
    <BootstrapSidebar
      :initial-show="initialShow"
      :links="links"
      :header="header"
      @sidebarChanged="onSidebarChanged"
    >
      <template v-slot:navbar>
        <b-navbar
          id="mainNavbar"
          toggleable="lg"
          type="light"
          variant="light"
          fixed="top"
        >
          <b-navbar-nav align="center">
            <b-nav-item><b>Award</b></b-nav-item> 
          </b-navbar-nav>
          <b-navbar-nav align="right" v-b-modal.modal-1>
            <b-nav-item><b><b-icon icon="filter"></b-icon></b></b-nav-item> 
          </b-navbar-nav>
        </b-navbar>
      </template>

      <template v-slot:content>
        <b-container style="margin-top: 56px">
          <router-view />
        </b-container>
      </template>
    </BootstrapSidebar>
  </div>
</template>

<style lang="scss">
@import "node_modules/vue-bootstrap-sidebar/src/scss/default-theme";
</style>

<style>
body {
  padding: 1rem;
}
</style>

<script>
import BootstrapSidebar from "vue-bootstrap-sidebar";
export default {
  components: {
    BootstrapSidebar,
  },
  data() {
    return {
      initialShow: false,
      header: "Awards Menu",
      links: [
        { name: "Home", href: { name: "Home" } },
        { name: "Logout", href: { name: "Login" } },
      ],
    };
  },
  methods: {
    onSidebarChanged() {
    },
  },
};
</script>