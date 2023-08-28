
<template>
  <div class="headerContainer">
    <header>
      <!-- <img v-if="showLogo !== undefined" :src="logoURL" />
    <slot /> -->

      
      <div>

        <div id="dropdown" :class="{ active: this.$store.state.show === 'more' }">
          <slot name="actions" />
        </div>

        <action v-if="this.$slots.actions" id="more" icon="more_vert" :label="$t('buttons.more')"
          @action="$store.commit('showHover', 'more')" />

        <div class="overlay" v-show="this.$store.state.show == 'more'" @click="$store.commit('closeHovers')" />

      </div>
      <div class="downloadButton">
        <button @click="refreshPage">
          <img src="../../icons/refresh.svg" alt="refresh" />
        </button>
      </div>

    </header>
    <div class="downloadBox">
      <input v-model="downloadLink" placeholder="https://civitai.com/api/download/models/143906" />
      <button @click="downloadFile">Download</button>
    </div>
  </div>
</template>

<script>
import { logoURL } from "@/utils/constants";
import { files as api } from "@/api";
import Action from "@/components/header/Action";

export default {
  name: "header-bar",
  props: ["showLogo", "showMenu"],
  components: {
    Action,
  },
  data: function () {
    return {
      logoURL,
      downloadLink: '',
    };
  },
  methods: {
    refreshPage() {
      this.$router.go();
    },
    openSidebar() {
      this.$store.commit("showHover", "sidebar");
    },
    downloadFile() {


      this.$toast.info('Downloading...');
      api.downloadFile(this.$route.path, this.downloadLink)
        .then(() => {
          this.$toast.success('Downloaded successfully!');
          this.$router.go();
        })
        .catch(error => {
          this.$toast.error('An error occurred while downloading! ');
          console.log(error);
        });

    },
  },
};
</script>

<style></style>


