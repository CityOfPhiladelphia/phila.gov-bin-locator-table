<template>
  <div>
    <div v-show="loading" class="mtm center">
      <i class="fas fa-spinner fa-spin fa-3x" />
    </div>
    <div v-show="!loading && empty" class="h3 mtm center">Sorry, there are no results.</div>
    <div v-show="failure" class="h3 mtm center">Sorry, there was a problem. Please try again.</div>
    <table v-show="!empty && !loading && !failure">
      <thead>
        <tr>
          <th class="name">Facility Name</th>
          <th class="address">Address</th>
          <th class="phone">Phone</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="location in binLocations" :key="location.OBJECTID">
          <td>{{ location.attributes.FacilityNa }}</td>
          <td>{{ location.attributes.Street_Add }}</td>
          <td>{{ location.attributes.Phone }} </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
import VueFuse from "vue-fuse";
import Vue from "vue";

const endpoint = "https://services.arcgis.com/fLeGjb7u4uXqeF9q/arcgis/rest/services/Recycling_Bin_Distribution_Centers/FeatureServer/0/query?f=pjson&where=Category2%20%3D%20%27Sanitation%20Convenience%20Center%27&returnGeometry=true&spatialRel=esriSpatialRelIntersects&outFields=*&maxRecordCountFactor=5&outSR=102100&resultOffset=0&resultRecordCount=5000&cacheHint=true&quantizationParameters=%7B%22mode%22%3A%22view%22%2C%22originPosition%22%3A%22upperLeft%22%2C%22tolerance%22%3A1.0583354500042268%2C%22extent%22%3A%7B%22xmin%22%3A2669670.2403021753%2C%22ymin%22%3A217797.0594086796%2C%22xmax%22%3A2744397.657776177%2C%22ymax%22%3A290085.5066950023%2C%22spatialReference%22%3A%7B%22wkid%22%3A102729%2C%22latestWkid%22%3A2272%7D%7D%7D";

Vue.use(VueFuse);

export default {
  name: "NeighborhoodResources",
  data: function() {
    return {
      binLocations: [],
      loading: false,
      failure: false,
      empty: false,
      currentSortDir: "desc",
    };
  },

  mounted() {
    this.fetchData();
  },

  filters: {

  },

  watch: {
    filteredResources() {
      if (!this.failure) {
        if (this.filteredResources.length == 0) {
          this.empty = true;
        } else {
          this.empty = false;
        }
      }
    }
  },

  methods: {
    fetchData: function() {
      this.loading = true;
      axios
        .get(endpoint)
        .then(response => {
          this.binLocations = response.data.features;
          this.failure = false;
          this.loading = false;
        })
        .catch(e => {
          console.log(e);
          this.loading = false;
          this.failure = true;
        });
    },

  }
};
</script>

<style scoped>
.phone{
  min-width: 150px;
}
</style>
