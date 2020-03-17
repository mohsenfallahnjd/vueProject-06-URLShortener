<template>
  <v-app id="app">
    <v-row class=" justify-center align-center">
      <v-col>
        <Shortner title="urlShortner.io" />
      </v-col>
    </v-row>
  </v-app>
</template>

<script>
import Shortner from "./components/Shortner";

export default {
  name: "App",
  components: {
    Shortner
  }
};
</script>
<style lang="scss">
#app {
  margin: 0 auto;
  background-color: #73cf5d;
}
</style>
