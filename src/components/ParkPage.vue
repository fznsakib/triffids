<template>
  <div>
    <Header v-bind:message="parkName" v-bind:hasBack="backLink"/>
    <div class="content">
      <Tmap v-if="park" :park="park" :drawerState="drawerState"/>
      <ParkDrawer @toggle-drawer="clicked" :park="park"/>
    </div>
  </div>
</template>

<script>
import Tmap from "./Tmap.vue";

import Header from "./Header.vue";
import ParkDrawer from "./ParkDrawer.vue";
import { parkService } from "../services/Park.service";

export default {
  name: "ParkPage",
  data: function() {
    return {
      drawerState: false,
      backLink: "/parks",
      park: null,
      parkName: ""
    };
  },
  beforeMount() {
    this.getPark().then(park => {
      this.park = park;
      this.parkName = park.siteName;
    });
  },

  methods: {
    async getPark() {
      const parkId = this.$route.params.parkId;
      let park = await parkService.park(parkId);
      return park;
    },

    clicked: function(event) {
      // event === true is drawer visible
      this.drawerState = event;
      if (event) {
        this.$log.info("Drawer opened");
      } else {
        this.$log.info("Drawer closed");
      }
    }
  },
  components: {
    Tmap,
    Header,
    ParkDrawer
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
