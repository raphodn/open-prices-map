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
    fetch('https://prices.openfoodfacts.org/data/locations.jsonl.gz')
    .then(res => res.arrayBuffer())
    // gz: ungzip
    // native alternative to pako? https://github.com/flameddd/blog/blob/master/2024-03-02%EF%BC%9AWeb%20JavaScript%2C%20How%20to%20fetch%20and%20read%20gzip%20JSON%20file%20with%20native%20API.md
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
