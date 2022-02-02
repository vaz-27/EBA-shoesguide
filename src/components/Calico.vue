<template>
  <q-page :style-fn="myTweak">
    <p
      :class="$q.screen.width > 1000 ? 'text-center' : 'text-justify'"
      style="margin-left: 0.8%; width:98%;"
    >
      Recomendado: São sapatos de diversos modelos, considerados confortáveis e bonitos nas cores preta, azul marinho, rosa, bege ou vermelha.
    </p>
    <img src="~assets/calico1.png" class="exemploGato" />
    <img src="~assets/calico2.png" class="exemploGato" />
    <img src="~assets/calico3.png" class="exemploGato" />
    <img src="~assets/calico4.png" class="exemploGato" />
    <img src="~assets/calico5.png" class="exemploGato" />
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
