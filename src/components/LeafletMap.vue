<template>
  <l-map ref="map" v-model:zoom="mapZoom" :center="mapCenter" :use-global-leaflet="false" @ready="initMap">
    <l-tile-layer :url="tiles" layer-type="base" name="OpenStreetMap" :attribution="attribution" />
    <l-marker v-for="location in locations" :key="location.id" :lat-lng="[location.osm_lat, location.osm_lon]">
      <l-popup>
        <h4>{{ location.osm_name }}</h4>
        <v-chip label size="small" density="comfortable">
          {{ location.osm_tag_key }}: {{ location.osm_tag_value }}
        </v-chip>
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
      default: () => [
        {id: 1, osm_name: 'Elefan', osm_tag_key: 'shop', osm_tag_value: 'convenience', osm_lat: 45, osm_lon: 5},
        {id: 2, osm_name: 'Intermarché', osm_tag_key: 'shop', osm_tag_value: 'supermarket', osm_lat: 46, osm_lon: 6},
      ]
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
