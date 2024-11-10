<template>
  <l-map ref="map" v-model:zoom="mapZoom" :center="mapCenter" :use-global-leaflet="false" @ready="initMap">
    <l-tile-layer :url="tiles" layer-type="base" name="OpenStreetMap" :attribution="attribution" />
    <l-marker v-for="location in locations" :key="location.id" :lat-lng="[location.osm_lat, location.osm_lon]">
      <l-popup>
        <h4>{{ location.osm_name }}</h4>
        <PriceCountChip :count="location.price_count" :withLabel="true" />
        <v-chip label size="small" density="comfortable">
          {{ location.osm_tag_key }}: {{ location.osm_tag_value }}
        </v-chip>
        <br />
        <a :href="'https://prices.openfoodfacts.org/locations/' + location.id" target="_blank">View in Open Prices</a>
      </l-popup>
    </l-marker>
  </l-map>
</template>

<script>
import 'leaflet/dist/leaflet.css'
import { LMap, LTileLayer, LMarker, LPopup } from '@vue-leaflet/vue-leaflet'

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
  },
  props: {
    locations: {
      type: Array,
      required: true,
      default: () => []
    },
  },
  data() {
    return {
      map: null,
      mapZoom: 5,
      mapCenter: [45, 5],
      mapBounds: null,
      tiles: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution: '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors'
    }
  },
  mounted() {
  },
  methods: {
    initMap() {  // TODO: improve map setup
      this.map = this.$refs.map.leafletObject
    },
  }
}
</script>
