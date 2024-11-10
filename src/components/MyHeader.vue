<template>
  <v-app-bar :elevation="1" class="bg-header">
    <v-app-bar-title>
      <span style="cursor:pointer" @click="$router.push('/')">
        <img src="/favicon.svg" height="28" width="28" style="vertical-align:bottom">
        Open Prices Map
      </span>
    </v-app-bar-title>
  </v-app-bar>
</template>
