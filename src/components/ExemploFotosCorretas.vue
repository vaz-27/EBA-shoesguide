<template>
  <q-page :style-fn="myTweak">
    <p class="text-justify" style="margin-left: 0.8%; width:98%;">
      Exemplos de como bater a foto:
    </p>
    <div class="text-center">
      <img src="~assets/foto_sim1.png" class="exemploFotos" /><br />
      <img src="~assets/foto_sim2.png" class="exemploFotos" /><br />
      <img src="~assets/foto_sim3.png" class="exemploFotos" /><br />
    </div>
  </q-page>
</template>
<script>
export default {
  methods: {
    myTweak(offset) {
      return { minHeight: null };
    }
  }
};
</script>
