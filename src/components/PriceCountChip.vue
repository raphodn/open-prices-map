<template>
  <v-chip label size="small" density="comfortable" :color="getColor()" class="mr-1">
    <v-icon start icon="mdi-tag-outline" />
    <span v-if="withLabel" id="price-count">{{ count }} prices</span>
    <span v-else id="price-count">{{ count }}</span>
  </v-chip>
</template>

<script>
export default {
  props: {
    count: {
      type: Number,
      default: null
    },
    withLabel: {
      type: Boolean,
      default: false
    },
  },
  methods: {
    getColor() {
      if (this.count === 0) {
        return 'red'  // 'error'
      }
      else if (this.count < 10) {
        return 'orange'  // 'warning'
      }
      else if (this.count < 50) {
        return 'orange'  // 'yellow' unreadable
      }
      else {  // >= 50
        return 'green'  // 'success'
      }
    },
  }
}
</script>
