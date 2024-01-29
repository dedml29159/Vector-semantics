<style src="vue-d3-network/dist/vue-d3-network.css"></style>
<template>
  <div id="app">
    <q-card class="chatmessages">
      <div class="flexrow">
        <div>
          <q-card-section>
            <div class="text-h6">
              Введите текст для анализа:
            </div>
          </q-card-section>
          <q-card-section>
            <q-input
            filled
            v-model="proc_text"
            type="textarea"
            autogrow
            float-label="Введите текст для анализа"
          /></q-card-section>
          <q-card-section>
            <q-btn color="primary" label="Обработать" @click="onProc" />
          </q-card-section>
        </div>
      </div>
      <q-card-section>
      <d3-network
      :net-nodes="nodes"
      :net-links="links"
      :options="options"
    ></d3-network></q-card-section>
    </q-card>
    <q-card class="chatmessages">
      <div class="flexrow">
        <div>
          <q-card-section>
            <div class="text-h6">Загрузка модели:</div>
          </q-card-section>
        </div>
        <div>
          <q-card-section>
            <q-uploader
              :url="hostae_uploadaem"
              label="Загрузите модель векторной семантики .model/*"
              square
              flat
              bordered
              single
              @uploaded="fileUploadedM"
              accept=".model, semanticModel/*"
              style="min-width: 900px; max-width: 900px"
            />
          </q-card-section>
        </div>
      </div>
    </q-card>
  </div>
</template>

<script>
import D3Network from "vue-d3-network";
import { ref } from "vue";
import axios from "axios";

export default {
  components: {
    D3Network,
  },
  data() {
    return {
      proc_text: ref("черный орех"),
      optionsT: [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1],
      threshold: ref(2),
      hostae_uploadaem: ref("http://192.168.3.73:5000/uploadaem"),
      nodes: ref([
        { id: 1, name: "a" },
        { id: 2, name: "b" },
      ]),
      links: ref([
        { sid: 1, tid: 2, name:'link 1'  },
        { sid: 2, tid: 1},
      ]),
      options: {
        force: 3000,
        nodeSize: 10,
        nodeLabels: true,
        linkLabels: true,
        linkWidth: 5,
      },
    };
  },
  methods: {
    fileUploadedM({ files, xhr }) {
      let data = JSON.parse(xhr.response);
      
    },

    async onProc() {
      console.log(this.nodes)
      console.log(this.links)
      const response = await axios({
        method: "post",
        url: "http://192.168.3.73:5000/getdata",
        data: {
          text: this.proc_text,
        },
        headers: {
          "Content-Type": "application/json",
        },
      });
      console.log('response');
      console.log(response);
      this.nodes = response.data.nodes;
      this.links = response.data.links;
    },
  }
};
</script>
<style lang="sass">
.page
  padding-bottom: 10px
  padding-top: 10px
  padding-left: 10px
  padding-r: 10px

.chatmessages
  padding: 10px
  margin: 10px
.flexrow
  display: flex
  flex-flow: row wrap
  justify-content: flex-start
.editorclass
  max-height: 500px
.inputtext
  width: 800px
</style>