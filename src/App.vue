<template>
  <v-app>

    <v-content>
      <router-view></router-view>
    </v-content>

  </v-app>
</template>

<script>
import { VApp, VContent } from 'vuetify/lib'

export default {
  name: 'App',
  components: {
    VApp,
    VContent
  },
  data: () => ({
    
  })
};
</script>

<style lang="scss">
@import '@/scss/_variables';
*,  *::before, *::after {
    box-sizing: border-box;
}

html {
    font-size: 16px;
}

#app {
  font-family: 'Inter', sans-serif;
  background-color: $body-background;
  margin: 0;
}

.router-link-exact-active, .router-link-active, a {
  text-decoration: none;
  color: inherit;
}

</style>