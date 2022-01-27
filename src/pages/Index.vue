<template>
  <q-page class="flex flex-center">
    <AnalisePelagemMobile v-show="displayMobile" />
    <AnalisePelagemDesktop v-show="displayDesktop" />
  </q-page>
</template>

<script>
import AnalisePelagemDesktop from "components/AnalisePelagemDesktop.vue";
import AnalisePelagemMobile from "components/AnalisePelagemMobile.vue";
export default {
  name: "PageIndex",
  data() {
    return {
      displayMobile: false,
      displayDesktop: false
    };
  },
  components: {
    AnalisePelagemDesktop,
    AnalisePelagemMobile
  },
  mounted() {
    if (
      window.navigator.userAgent.match(/Android/i) ||
      window.navigator.userAgent.match(/iPhone/i) ||
      window.navigator.userAgent.match(/iPad/i)
    ) {
      this.displayMobile = true;
      this.displayDesktop = false;
    } else {
      this.displayMobile = false;
      this.displayDesktop = true;
    }
  }
};
</script>
