<template>
  <v-app dark>
    <v-navigation-drawer :mini-variant="isMiniVariant">
      <v-list>
      </v-list>
    </v-navigation-drawer>
    <v-content>
      <router-view></router-view>
    </v-content>
    <v-footer>
    </v-footer>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      isMiniVariant: false
    }),
    methods: {
    }
  }
</script>

<style>
</style>
