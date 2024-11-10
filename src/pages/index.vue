<template>
  <LeafletMap :locations="locations" />
</template>

<script setup>
  import { ref, onMounted } from 'vue'
  import pako from 'pako'

  let locations = ref([])
  let loading = ref(false)

  onMounted(() => {
    loading = true
    fetch('https://cors-anywhere.herokuapp.com/https://prices.openfoodfacts.org/data/locations.jsonl.gz')
    .then(res => res.arrayBuffer())
    // gz: ungzip
    .then(buf => pako.inflate(buf, {to: 'string'}))
    // jsonl: parse line by line
    .then(data => {
      const lines = data.split(/\n/)
      lines.forEach(line => {
        if (line) {  // skip empty lines
          const location = JSON.parse(line)
          if (location.osm_lat && location.osm_lon) {  // skip locations without coordinates (e.g. "ONLINE")
            locations.value.push(location)
          }
        }
        loading = false
      })
    })
  })
</script>
