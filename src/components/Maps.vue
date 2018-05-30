<template>
  <div>
    <div class="container" style="margin-top:20px;">
      <p class="content is-medium">Find the specific map by searching below.</p>
      <div class="columns">
        <search-box v-model="filterQuery" placeholder="Find a Map" />
      </div>
      <div class="cards">
        <app-map-item v-for='map in displayedMaps' :key='map.id' :mapInfo='map'></app-map-item>
      </div>
    </div>
  </div>
</template>
<script>
import SearchBox from "./SearchBox.vue";
import mapItem from "./MapItem.vue";
import axios from "axios";
import config from "../config";

export default {
  data() {
    return {
      maps: [],
      filterQuery: ""
    };
  },
  components: {
    "app-map-item": mapItem,
    "search-box": SearchBox
  },
  computed: {
    displayedMaps() {
      const maps = this.maps;
      const filteredMaps = maps.filter(this.filterMaps);
      return filteredMaps;
    }
  },
  methods: {
    filterMaps(map) {
      const strTags = map.tags.join(" ").toLowerCase();
      const lowercaseName = map.title.toLowerCase();
      const description = map.description ? map.description.toLowerCase() : '';
      const selection = strTags + lowercaseName + description;
      const lowercaseQuery = this.filterQuery.toLowerCase();
      if(selection.indexOf(lowercaseQuery) > -1 && map.thumbnail){
        return true;
      }else{
        return false;
      }
    }
  },
  created() {
    axios
      .get(config.getUrl())
      .then(response => {
        this.maps = response.data.results;
      })
      .catch(error => console.log(error));
  }
};
</script>
<style>
.is-medium {
  text-align: center;
}

.control {
  margin-right: 0.5em;
}

.cards {
  display: -webkit-flex;
  display: flex;
  -webkit-justify-content: center;
  justify-content: center;
  -webkit-flex-wrap: wrap;
  flex-wrap: wrap;
  padding: 1.5%;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

.card {
  position: relative;
  margin-bottom: 30px;
  background: #fefff9;
  color: #363636;
  text-decoration: none;
  -moz-box-shadow: rgba(0, 0, 0, 0.19) 0 0 2px 0;
  -webkit-box-shadow: rgba(0, 0, 0, 0.19) 0 0 2px 0;
  box-shadow: rgba(0, 0, 0, 0.19) 0 0 2px 0;
  -moz-border-radius: 4px;
  -webkit-border-radius: 4px;
  border-radius: 4px;
  border: 2px solid lightgray;
}

@media (max-width: 700px) {
  .card {
    width: 100%;
  }
  .esri-ui-bottom-right {
    display: none;
  }
}

@media (min-width: 700px) {
  .card {
    margin-right: 20px;
    margin-bottom: 20px;
  }
}

@media (min-width: 1010px) {
  .card {
    max-width: 320px;
    margin-right: 30px;
    margin-bottom: 30px;
  }
}

.card span {
  display: block;
}

.card .card-summary {
  padding: 5% 5% 3% 5%;
}

.card .card-header {
  position: relative;
  height: 175px;
  overflow: hidden;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  background-color: rgba(255, 255, 255, 0.15);
  background-blend-mode: overlay;
  -moz-border-radius: 4px 4px 0 0;
  -webkit-border-radius: 4px;
  border-radius: 4px 4px 0 0;
}

.card .card-header:hover,
.card .card-header:focus {
  background-color: rgba(255, 255, 255, 0);
}

.card .card-title {
  background: rgba(49, 54, 60, 0.85);
  padding: 3.5% 0 2.5% 0;
  color: white;
  font-family: "Roboto Condensed", sans-serif;
  text-transform: uppercase;
  position: absolute;
  bottom: 0;
  width: 100%;
}

.card .card-title h3 {
  font-size: 1.2em;
  line-height: 1.2;
  padding: 0 3.5%;
  margin: 0;
}

.card:hover,
.card:focus {
  background: white;
  -moz-box-shadow: rgba(0, 0, 0, 0.45) 0px 0px 20px 0px;
  -webkit-box-shadow: rgba(0, 0, 0, 0.45) 0px 0px 20px 0px;
  box-shadow: rgba(0, 0, 0, 0.45) 0px 0px 20px 0px;
}

.card:hover .card-title,
.card:focus .card-title {
  background: rgba(71, 93, 4, 0.9);
}

* {
  -moz-transition-property: background-color, border-color, box-shadow, color,
    opacity, text-shadow, -moz-transform;
  -o-transition-property: background-color, border-color, box-shadow, color,
    opacity, text-shadow, -o-transform;
  -webkit-transition-property: background-color, border-color, box-shadow, color,
    opacity, text-shadow, -webkit-transform;
  transition-property: background-color, border-color, box-shadow, color,
    opacity, text-shadow, transform;
  -moz-transition-duration: 0.2s;
  -o-transition-duration: 0.2s;
  -webkit-transition-duration: 0.2s;
  transition-duration: 0.2s;
  -moz-transition-timing-function: linear;
  -o-transition-timing-function: linear;
  -webkit-transition-timing-function: linear;
  transition-timing-function: linear;
}

.footer {
  padding-top: 1.5rem;
}
</style>
