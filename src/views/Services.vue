<template>
  <v-container fluid>
    <search-bar />
    <v-row dense>
      <v-col cols="4">
        <service-tabs :serviceTab="tab_services" @selectionUpdate="captureSelectionUpdate" />
      </v-col>

      <v-col cols="8">
        <service-detail-tabs :search_input="searchInput" :tab_services="tab_services" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import ServiceTabs from "../components/service/ServiceTabs";
import ServiceDetailTabs from "../components/service/detail/ServiceDetailTabs";
import SearchBar from "../components/SearchBar";
import { mapState, mapActions } from "vuex";

export default {
  components: {
    SearchBar,
	ServiceTabs,
	ServiceDetailTabs
  },
  data: () => ({
    pnl_search: 0,
    tab_services: -1,

    admins: {},
  }),
  computed: {
    ...mapState("services", ["searchInput", "serviceSelection"]),
  },
  mounted() {
    if (this.$route.params.search.length === 1) {
      this.tab_services = 0;
    } else {
      this.tab_services = 3;
    }
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.resize);
  },
  watch: {
    tab_services: {
      handler() {
        this.updateServiceSelection([]);
      },
    },
    searchInput: {
      deep: true,
      handler() {
        this.updateServiceSelection([]);
        this.tab_services = 3;
      },
    },
  },
  methods: {
    ...mapActions("services", ["updateServiceSelection"]),
    resize() {
      let elements = this.getElementsByClassName("testitest");
      for (let i = 0; i < elements; i++) {
        elements[i].setAttribute(
          "style",
          "min-height:400px; height:" +
            (window.innerHeight - elements[i].getBoundingClientRect().top) +
            "px;"
        );
      }
    },
    getElementsByClassName(className) {
      if (document.getElementsByClassName) {
        return document.getElementsByClassName(className);
      } else {
        return document.querySelectorAll("." + className);
      }
    },
    recieveSearchInput(newInput) {
      this.search_input = newInput;
    },
    captureSelectionUpdate(data) {
		console.log('UPDATE SELCTION DATA '+data);
      this.tab_services = data;
    },
  },
};
</script>

<style>
.testitest {
  /*min-height: 600px;*/
  /*height: 100%;*/
}
</style>
