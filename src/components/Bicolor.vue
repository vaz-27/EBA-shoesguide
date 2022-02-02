<template>
  <q-page :style-fn="myTweak">
    <p
      :class="$q.screen.width > 1000 ? 'text-center' : 'text-justify'"
      style="margin-left: 0.8%; width:98%;"
    >
      Não recomendado: São sapatos de diversos modelos, desconfortáveis nas cores amarelo, roxo, verde, laranja e zebra.
    </p>
    <img src="~assets/bicolor1.png" class="exemploGato" />
    <img src="~assets/bicolor2.png" class="exemploGato" />
    <img src="~assets/bicolor3.png" class="exemploGato" />
    <img src="~assets/bicolor4.png" class="exemploGato" />
    <img src="~assets/bicolor5.png" class="exemploGato" />
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
