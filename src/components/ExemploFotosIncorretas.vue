<template>
  <q-page :style-fn="myTweak">
    <p>
      Exemplos de como não bater a foto:
    </p>
    <div class="text-center">
      <img src="~assets/nao_foto1.jpg" class="exemploFotos" /><br />
      <img src="~assets/nao_foto2.jpg" class="exemploFotos" /><br />
      <img src="~assets/nao_foto3.jpg" class="exemploFotos" /><br />
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
