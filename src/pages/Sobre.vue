<template>
  <q-page class="flex flex-center">
    <div class="text-h5 text-primary q-ma-sm">Sobre o Aplicativo</div>
    <div style="width: 100%; padding-bottom: 135%;">
      <q-list>
        <q-expansion-item
            label="Tutorial sobre como bater a foto"
            group="tutorial_bater_foto"
            class="bg-secondary text-justify"
            :default-opened="$q.screen.width > 1000"
          >
          <q-card>
            <q-card-section class="text-dark text-center">
              <p class="text-justify">A foto deve ser feita com iluminação branca.</p>
              <exemploFotosCorretas /><br>
              <exemploFotosIncorretas />
            </q-card-section>
          </q-card>
        </q-expansion-item>
        <q-expansion-item label="Processo de criação" group="processo_criacao"
 :class="$q.screen.width > 1000 ? 'bg-secondary text-justify' : 'text-justify'"
          :default-opened="$q.screen.width > 1000">
        <q-card>
          <q-card-section class="text-dark text-justify">
            <p>O App EBA-shoesguide consiste em um exemplo de identificação de sapatos utilizando rede neural convolucional com transferência de aprendizado (MobileNetV2 - Imagenet).</p>
            <p>Este aplicativo foi construído com o intuito de auxiliar mulheres com deficiência visual a encontrarem sapatos bonitos de acordo com os critérios dos desenvolvedores. Os sapatos nas cores preto, vermelho, beje, azul marinho e rosa são recomendados e nas cores roxo, amarelo, verde, laranja e zebra são não recomendados.</p>
            <p>
              Para elaborar esta aplicação foi utilizado as seguintes ferramentas e
              bibliotecas: Python, TensorflowJS, Tensorflow, Quasar, VueJS e
              Cordova.
            </p>
            <p>
              A primeira etapa do projeto foi a construção de 1 robô para captura de imagens de sapatos utilizando a API do Google.
            </p>
            <p>
              Na segunda etapa, realizamos o treinamento do modelo em
              Python, utilizando a extensão H5 para salvar o modelo treinado.
              Em seguida, foi feita conversão do modelo gerado para JSON 
              através do TensorflowJS Converter.
            </p>
            <p>
                Na terceira etapa foi feita a importação do modelo JSON para dentro do TensorflowJS e a integração do mesmo com o framework Quasar.
            </p>
             <p>
                Por último, geramos um aplicativo para a plataforma Android utilizando a tecnologia Cordova que já vem integrada no framework Quasar.
            </p>
            <p>
              O treino da rede e o modelo salvo estão disponíveis em:   
              <a
                href="#"
                v-on:click="
                  abrirLink('https://colab.research.google.com/drive/1LiuthDjPiwhXHDLseTGcpP5Kd2BVd3Rw')
                "
                >https://colab.research.google.com/drive/1LiuthDjPiwhXHDLseTGcpP5Kd2BVd3Rw</a
              >
            </p>
            <p>
              O projeto de desenvolvimento do app no Quasar está disponível através do
              seguinte link: 
              <a
                href="#"
                v-on:click="
                  abrirLink('https://github.com/vaz-27/EBA-shoesguide')
                "
                >https://github.com/vaz-27/EBA-shoesguide</a
              >
            </p>
            <p>
              A documentação para desenvolver o aplicativo com o Cordova está
              disponível no seguinte link:
              <a
                href="#"
                v-on:click="
                  abrirLink(
                    'https://quasar.dev/quasar-cli/developing-cordova-apps/preparation'
                  )
                "
                >Developing Cordova Apps - Preparation</a
              >
            </p>
            <p>
             Este projeto foi realizado pelos alunos do curso de Tecnologia em Informática em Saúde - TS12 - UNIFESP, na unidade curricular Projeto Integrador:
              <ul>
                <li>Alessandra Felix Cardoso.</li>
                <li>Beatriz Maria Gonçalves Vaz.</li>
                <li>Eduardo Loeffel Noce Gobi.</li>
              </ul>
              Com a colaboração de:
              <ul>
                <li>Antonio Carlos da Silva Junior: Analista de Tecnologia da Informação no Departamento de Informática em Saúde da UNIFESP.</li>
                <li>Ivan Calixto Ribeiro - Departamento de Informática em Saúde da UNIFESP.</li>
              </ul>
            </p>
          </q-card-section>
        </q-card>
      </q-expansion-item>
     <!--<q-expansion-item
        label="Explicação sobre tipos de pelagem"
        group="explicacao_tipos_pelagem"
        class="bg-secondary text-justify"
        :default-opened="$q.screen.width > 1000"
      >
        <q-card>
          <q-card-section class="text-dark text-center">
            <bicolor />
            <calico />
          </q-card-section>
        </q-card>
      </q-expansion-item>-->
      <q-expansion-item label="Links externos" group="links_externos"
 :class="$q.screen.width > 1000 ? 'bg-secondary text-justify' : 'text-justify'"
          :default-opened="$q.screen.width > 1000">
        <q-card>
          <q-card-section class="text-dark text-justify">
            <p>
              <a
                href="#"
                v-on:click="
                  abrirLink('https://keras.io/api/applications/mobilenet/')
                "
                >Documentação MobileNet V2</a
              >
            </p>
            <p>
              <a
                href="#"
                v-on:click="
                  abrirLink(
                    'https://www.youtube.com/watch?v=mu2j0rccrlg'
                  )
                "
                >Dicas de utilização de celular por deficientes visuais.</a
              >
            </p>
            <!--<p>
              <a
                href="#"
                v-on:click="
                  abrirLink(
                    'https://meowbarkers.wordpress.com/2015/08/30/tabby-patterns-and-colors-cats/'
                  )
                "
                >Links sobre padrões e cores dos gatos "tabby"</a
              >
            </p>
            <p>
              <a
                href="#"
                v-on:click="
                  abrirLink(
                    'https://www.omlet.us/guide/cats/choosing_the_right_cat_for_you/cat_coat_types'
                  )
                "
                >Links sobre tipos de pelagens de gatos</a
              >
            </p>
            <p>
              <a
                href="#"
                v-on:click="
                  abrirLink(
                    'https://silvershorthairs.com/2017/09/23/featured-content/'
                  )
                "
                >Links sobre tipos de gatos "tabby"</a
              >
            </p>
            <p>
              <a
                href="#"
                v-on:click="
                  abrirLink(
                    'http://cedarseed.com/portfolio/housecat-colors/'
                  )
                "
                >Links sobre combinações e tipos de pelagem - Joumana Medlej</a
              >
            </p>-->
          </q-card-section>
        </q-card>
      </q-expansion-item>
    </q-list>
    </div>
  </q-page>
</template>
<style>
  .exemploFotos {
    width: 200px;
    display: inline-block;
  }
</style>
<script>
import { openURL } from "quasar";
import bicolor from "components/Bicolor.vue";
import calico from "components/Calico.vue";
import exemploFotosIncorretas from "components/ExemploFotosIncorretas.vue";
import exemploFotosCorretas from "components/ExemploFotosCorretas.vue";
 
export default {
  name: "Sobre",
 
  methods: {
    abrirLink(url) {
      openURL(url);
    }
  },
  components: {
    /*tabbySpotted,
    tabbyMackerel,
    tabbyClassic,
    solidColor,
    pointColor,
    hairless,
    bicolor,
    calico,
    tortoiseShell,*/
    exemploFotosIncorretas,
    exemploFotosCorretas
  }
};
</script>