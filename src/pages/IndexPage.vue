<template>
  <div class="app-header">
    <template v-if="isLoading"> Loading... </template>

    <template v-else>
      <span v-if="showAllPages"> {{ pageCount }} page(s) </span>

      <span v-else>
        <button :disabled="page <= 1" @click="page--">❮</button>

        {{ page }} / {{ pageCount }}

        <button :disabled="page >= pageCount" @click="page++">❯</button>
      </span>

      <label class="right">
        <input v-model="showAllPages" type="checkbox" />

        Show all pages
      </label>
    </template>
  </div>

  <div class="app-content">
    <div class="blokje"><input type="text" /></div>
    <vue-pdf-embed
      ref="pdfRef"
      :source="pdfSource"
      :page="page"
      @password-requested="handlePasswordRequest"
      @rendered="handleDocumentRender"
    />
  </div>
</template>

<script>
import VuePdfEmbed from 'vue-pdf-embed';

// OR THE FOLLOWING IMPORT FOR VUE 2
// import VuePdfEmbed from 'vue-pdf-embed/dist/vue2-pdf-embed'

export default {
  components: {
    VuePdfEmbed,
  },
  data() {
    return {
      isLoading: true,
      page: null,
      pageCount: 1,
      pdfSource: 'src/assets/test.pdf',
      showAllPages: false,
    };
  },
  watch: {
    showAllPages() {
      this.page = this.showAllPages ? null : 1;
    },
  },
  methods: {
    handleDocumentRender(args) {
      console.log(args);
      this.isLoading = false;
      this.pageCount = this.$refs.pdfRef.pageCount;
    },
    handlePasswordRequest(callback, retry) {
      callback(prompt(retry ? 'Enter password again' : 'Enter password'));
    },
  },
};
</script>
<style>
.blokje {
  position: absolute;
  z-index: 20;
  top: 2cm;
  left: 3cm;
  width: 2cm;
  height: 2cm;
  border: 2px solid blue;
  fill: gold;
}
</style>
