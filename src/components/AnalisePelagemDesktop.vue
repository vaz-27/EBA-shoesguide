<template>
  <q-page class="flex flex-center">
    <div class="full-width text-center">
      <img
        :src="img"
        alt="image"
        id="fotoDesktop"
        class="photo"
        style="object-fit:cover;"
        v-show="displayFoto"
      />
      <video
        id="video"
        class="desktop-only text-center photo"
        style="object-fit:cover;"
        v-show="displayVideo"
      >
        Vídeo não disponível.
      </video>
      <canvas
        id="canvas"
        style="object-fit:cover;"
        class="photo"
        v-show="displayCanvas"
      />
    </div>
    <div class="text-center">
      <q-file
        class="QFileGato"
        label="selecione uma foto de sapato"
        outlined
        v-model="arquivo"
        @input="carregarImagem"
        v-show="displayQFile"
        accept=".jpg, image/*"
        ><template v-slot:prepend> </template>
      </q-file>
    </div>
    <div id="divBotoes" class="full-width text-center q-gutter-sm">
      <q-btn
        id="btnTirarFoto"
        color="primary"
        @click="tirarFoto()"
        v-show="btnTirarFoto"
        >Tirar foto</q-btn
      >
      <q-btn
        id="btnAtivarVideo"
        color="primary"
        class="desktop-only"
        @click="ativarVideo()"
        v-show="btnAtivarVideo"
        >Ativar Vídeo</q-btn
      >
      <q-btn
        id="btnEncerrarVideo"
        color="primary"
        class="desktop-only"
        v-show="btnEncerrarVideo"
        @click="encerrarVideo()"
        >Encerrar Vídeo</q-btn
      >
      <q-btn
        id="btnLimparCampos"
        color="primary"
        @click="limparCampos()"
        v-show="btnLimparCampos"
        >Limpar Campos</q-btn
      >
    </div>
    <div id="msg" v-html="msgPredicao" class="full-width textoPredito"></div>
    <div
      id="explicacaoPelagens"
      class="full-width"
      v-show="divExplicacaoPelagens"
    >
      <bicolor class="full-width text-center" v-show="explicacaoBicolor" />
      <calico class="full-width text-center" v-show="explicacaoCalico" />
    </div>
    <q-list
      id="listaCompletaProbabilidades"
      style="margin-left: 0.8%; width:98%;"
    >
      <q-expansion-item
        label="Lista completa das probabilidades"
        group="listaProbabilidades"
        class="bg-secondary"
      >
        <q-card>
          <div
            v-html="listaCompletaProbabilidades"
            style="margin-left:5px;"
            class="text-justify"
          ></div>
        </q-card>
      </q-expansion-item>
    </q-list>
  </q-page>
</template>
<style scoped>
.photo {
  height: 244px;
  width: 244px;
}

.textoPredito {
  white-space: pre-wrap;
  text-align: center;
  height: 30px;
}
.QFileGato {
  width: 244px;
}
</style>
<script>
import * as tf from "@tensorflow/tfjs";
import { fetch as fetchPolyfill } from "whatwg-fetch";
import logo from "assets/logo.png";
import bicolor from "components/Bicolor.vue";
import calico from "components/Calico.vue";
export default {
  data() {
    return {
      height: 244,
      width: 244,
      msgPredicao: "Modelo carregado",
      modelo: tf.sequential(),
      labels: [
        "Bicolor",
        "Calico"
      ],
      objetoPredicao: "",
      img: logo,
      arquivo: null,
      explicacaoBicolor: false,
      explicacaoCalico: false,
      listaCompletaProbabilidades: "",
      canvas: "",
      stream: null,
      displayFoto: true,
      displayVideo: false,
      displayCanvas: false,
      displayCapturaTela: false,
      btnAtivarVideo: true,
      btnEncerrarVideo: false,
      btnTirarFoto: false,
      divExplicacaoPelagens: false,
      displayQFile: true,
      btnLimparCampos: false
    };
  },
  mounted() {
    try {
      window.fetch = fetchPolyfill;
      this.limparExplicacao();
      this.carregarModelo();
    } catch (error) {
      alert(error);
    }

    this.canvas = document.getElementById("canvas");
    this.canvas.width = video.width = this.width;
    this.canvas.height = video.width = this.height;
  },
  methods: {
    async carregarModelo() {
      this.msgPredicao = "Carregando modelo ...";
      try {
        this.modelo = await tf.loadLayersModel("model/model.json");
      } catch (error) {
        alert(error);
      }
      this.msgPredicao = "Modelo carregado";
    },
    limparCampos() {
      this.img = logo;
      this.arquivo = null;
      this.objetoPredicao = "";
      this.listaCompletaProbabilidades = "";
      this.msgPredicao = "Tire uma foto ou adicione uma imagem";
      this.btnLimparCampos = false;
      this.limparExplicacao();

      if (this.stream != null) {
        this.encerrarVideo();
      }
    },
    limparExplicacao() {
      this.explicacaoBicolor = false;
      this.explicacaoCalico = false;
      this.divExplicacaoPelagens = false;
      this.listaCompletaProbabilidades = "";
    },
    ativarVideo() {
      this.displayFoto = false;
      this.displayVideo = true;
      this.displayCanvas = false;
      this.btnAtivarVideo = false;
      this.btnTirarFoto = true;
      this.btnEncerrarVideo = true;
      this.arquivo = null;
      this.limparExplicacao();
      this.msgPredicao = "Clique em tirar foto";
      this.displayQFile = false;
      this.btnLimparCampos = false;

      this.stream = navigator.mediaDevices
        .getUserMedia({
          audio: false,
          video: true
        })
        .then(function(stream) {
          video.srcObject = stream;
          video.play();
          return stream;
        })
        .then(stream => {
          this.stream = stream;
        })
        .catch(function(err) {
          console.log("An error occurred: " + err);
        });
    },
    alertaCarregando(param) {
      if (param == true) {
        this.$q.loading.show({
          spinnerColor: "primary",
          spinnerSize: 140,
          backgroundColor: "white",
          message: "Carregando... Por favor aguarde",
          messageColor: "black"
        });
      } else if (param == false) {
        this.$q.loading.hide();
      }
    },
    tirarFoto() {
      var context = canvas.getContext("2d");

      context.drawImage(video, 0, 0, canvas.width, canvas.height);
      var data = canvas.toDataURL("image/png");
      this.img = data;
      this.displayFoto = true;
      this.displayVideo = false;
      this.displayCanvas = false;
      this.btnAtivarVideo = false;
      this.btnTirarFoto = false;
      this.btnEncerrarVideo = true;
      this.divExplicacaoPelagens = true;
      this.predizerImagem();
      this.btnLimparCampos = false;
    },
    encerrarVideo() {
      this.displayVideo = false;
      this.displayCanvas = false;
      this.displayFoto = true;
      this.btnAtivarVideo = true;
      this.btnTirarFoto = false;
      this.btnEncerrarVideo = false;
      this.img = logo;
      this.msgPredicao = "Modelo carregado";
      this.displayQFile = true;
      this.arquivo = null;
      this.limparExplicacao();

      document.getElementById("video").srcObject = null;

      this.stream.getTracks().forEach(function(track) {
        if (track.kind == "video") {
          track.stop();
          track.enabled = false;
        }
      });
    },
    predizerImagem() {
      this.divExplicacaoPelagens = true;
      this.msgPredicao = "Reconhecendo pelagem aguarde...";
      this.objetoPredicao = document.getElementById("fotoDesktop");
      //console.log("objeto ", this.objetoPredicao);

      let arrInput = tf.browser.fromPixels(this.objetoPredicao);
      this.objetoPredicao = tf.image
        .resizeBilinear(arrInput, [224, 224])
        .reshape([1, 224, 224, 3])
        .div(tf.scalar(255));

      this.alertaCarregando(true);

      setTimeout(() => {
        let valor = "";
        try {
          valor = this.modelo.predict(this.objetoPredicao);
          //console.log("valor ", valor.argMax(-1));
        } catch (error) {
          alert(error);
        }

        this.msgPredicao =
          "<b>" +
          this.labels[valor.argMax(-1).dataSync()[0]] +
          "</b>: " +
          (valor.dataSync()[valor.argMax(-1).dataSync()[0]] * 100).toFixed(4) +
          "%";

        switch (valor.argMax(-1).dataSync()[0]) {
          case 0:
            this.explicacaoBicolor = true;
            break;
          case 1:
            this.explicacaoCalico = true;
            break;
        }

        this.listaCompletaProbabilidades = "";
        valor.dataSync().forEach((element, index) => {
          let percent = (element * 100).toFixed(4);
          this.listaCompletaProbabilidades +=
            " " + percent + "% - " + this.labels[index] + "<br>";
        });
        this.alertaCarregando(false);
      }, 500);

      this.btnLimparCampos = true;
    },
    carregarImagem() {
      this.displayFoto = true;
      this.displayVideo = false;
      this.displayCanvas = false;

      //this.objetoPredicao = "";
      this.img = URL.createObjectURL(this.arquivo);

      this.msgPredicao = "Reconhecendo pelagem aguarde...";
      this.limparExplicacao();
      setTimeout(() => {
        this.predizerImagem();
      }, 500);
    }
  },
  components: {
    bicolor,
    calico,
  }
};
</script>
