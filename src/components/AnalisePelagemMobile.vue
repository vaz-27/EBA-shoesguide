<template>
  <q-page class="flex flex-center">
    <div class="full-width text-center" style="margin-top: 1%;">
      <img :src="img" alt="Imagem selecionada" id="foto" class="photo" />
    </div>
    <div class="text-center">
      <q-file
        class="QFileGato"
        label="Selecione uma foto de sapato"
        outlined
        v-model="arquivo"
        @input="carregarImagem"
        accept=".jpg, image/*"
        ><template v-slot:prepend> </template>
      </q-file>
    </div>
    <div id="divBotoes" class="full-width text-center q-gutter-sm">
      <q-btn id="btnTirarFoto" color="primary" @click="tirarFoto()"
        >Tirar foto</q-btn
      >
      <q-btn
        id="btnLimparCampos"
        color="primary"
        @click="limparCampos()"
        v-show="btnLimparCampos"
        >Limpar Campos</q-btn
      >
    </div>
    <div id="msg" v-html="msgPredicao" class="textoPredito"></div>
    <div id="explicacaoPelagens" class="full-width text-center">
      <bicolor class="full-width text-center" v-show="explicacaoBicolor" />
      <calico class="full-width text-center" v-show="explicacaoCalico" />
    </div>
    <q-list id="listaCompletaProbabilidades" class="full-width text-center">
      <q-expansion-item
        label="Lista completa das probabilidades"
        group="listaProbabilidades"
        class="bg-secondary"
        style="text-align:left;"
      >
        <q-card>
          <div
            v-html="listaCompletaProbabilidades"
            class="text-justify"
            style="text-align: left; margin-left:2%;"
          ></div>
        </q-card>
      </q-expansion-item>
    </q-list>
  </q-page>
</template>
<style scoped>
.photo {
  height: 224px;
  width: 224px;
}
.QFileGato {
  width: 224px;
}
.textoPredito {
  white-space: pre-wrap;
  text-align: center;
  height: 3%;
  margin-top: 2%;
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
      height: 224,
      width: 224,
      msgPredicao: "Tire uma foto do gato",
      modelo: tf.sequential(),
      labels: [
        "Não Recomendado",
        "Recomendado"
      ],
      objetoPredicao: "",
      img: logo,
      arquivo: null,
      explicacaoBicolor: false,
      explicacaoCalico: false,
      explicacaoTortoiseShell: false,
      listaCompletaProbabilidades: "",
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
  },
  methods: {
    async carregarModelo() {
      this.msgPredicao = "Carregando Modelo ...";
      try {
        this.modelo = await tf.loadLayersModel("model/model.json");
      } catch (error) {
        alert(error);
      }
      this.msgPredicao = "Modelo Carregado";
    },
    limparCampos() {
      this.img = logo;
      document.getElementById("foto").src = logo;
      this.arquivo = null;
      this.objetoPredicao = "";
      this.listaCompletaProbabilidades = "";
      this.msgPredicao = "Tire uma foto ou adicione uma imagem";
      this.btnLimparCampos = false;
      this.limparExplicacao();
    },
    limparExplicacao() {
      this.explicacaoBicolor = false;
      this.explicacaoCalico = false;
      this.listaCompletaProbabilidades = "";
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
      try {
        let opts = {
          quality: 100,
          destinationType: Camera.DestinationType.FILE_URI,
          sourceType: Camera.PictureSourceType.CAMERA,
          mediaType: Camera.MediaType.PICTURE,
          encodingType: Camera.EncodingType.JPEG,
          cameraDirection: Camera.Direction.BACK,
          correctOrientation: true,
          targetWidth: this.width,
          targetHeight: this.height
        };
        navigator.camera.getPicture(
          imgURI => {
            document.getElementById("foto").src = imgURI;
            this.limparExplicacao();
            this.msgPredicao = "Reconhecendo pelagem aguarde...";
            setTimeout(() => {
              this.predizerImagem();
            }, 500);
          },
          msg => {
            this.msgPredicao = msg;
          },
          opts
        );
      } catch (error) {
        console.log(error);
      }

      this.btnLimparCampos = true;
    },
    predizerImagem() {
      this.objetoPredicao = document.getElementById("foto");

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
        this.btnLimparCampos = true;
      }, 500);
    },
    carregarImagem() {
      this.img = URL.createObjectURL(this.arquivo);

      this.msgPredicao = "Reconhecendo sapato aguarde...";
      this.limparExplicacao();
      setTimeout(() => {
        this.predizerImagem();
      }, 500);
    }
  },
  components: {
    bicolor,
    calico
  }
};
</script>
