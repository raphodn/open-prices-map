<template>
  <l-map ref="map" v-model:zoom="mapZoom" :center="mapCenter" :use-global-leaflet="false" :options="mapOptions" @ready="initMap">
    <l-tile-layer :url="tiles" layer-type="base" name="OpenStreetMap" :attribution="attribution" />
    <l-control position="bottomright">
      <div style="color:black;padding:10px;background-color:white;border-radius:10px;">
        <h4 style="margin-bottom:5px">Number of prices at the store</h4>
        <i style="width:15px;height:15px;margin-right:5px;background-color:#72b043;display:inline-block;opacity:0.8"></i> More than 50<br>
        <i style="width:15px;height:15px;margin-right:5px;background-color:#f8cc1b;display:inline-block;opacity:0.8"></i> Between 10 and 50<br>
        <i style="width:15px;height:15px;margin-right:5px;background-color:#f37324;display:inline-block;opacity:0.8"></i> Less than 10<br>
        <i style="width:15px;height:15px;margin-right:5px;background-color:#e12729;display:inline-block;opacity:0.8"></i> 0 prices
      </div>
    </l-control>
    <l-marker v-for="location in locations" :key="location.id" :lat-lng="[location.osm_lat, location.osm_lon]">
      <l-icon :icon-url="getLocationIconUrl(location)" :icon-size="[25, 41]" :icon-anchor="[12, 41]" :popup-anchor="[1, -34]" :shadow-size="[41, 41]" />
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
import { LMap, LTileLayer, LMarker, LPopup, LIcon, LControl } from '@vue-leaflet/vue-leaflet'

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LIcon,
    LControl,
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
      mapZoom: 3,
      mapCenter: [28, 10],
      mapBounds: null,
      mapOptions: {
        preferCanvas: true,  // doesn't work, see https://github.com/vue-leaflet/vue-leaflet/issues/199
      },
      tiles: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution: '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors'
      // icons from https://github.com/pointhi/leaflet-color-markers
    }
  },
  mounted() {
  },
  methods: {
    initMap() {  // TODO: improve map setup
      this.map = this.$refs.map.leafletObject
    },
    getLocationIconUrl(location) {
      // color palette from https://colorkit.co/palette/e12729-f37324-f8cc1b-72b043-007f4e/
      if (location.price_count === 0) {
        return '/marker-icon-red.png'
      }
      else if (location.price_count < 10) {
        return '/marker-icon-orange.png'
      }
      else if (location.price_count < 50) {
        return '/marker-icon-yellow.png'
      }
      else {  // >= 50
        return '/marker-icon-green.png'
      }
    },
  }
}
</script>
