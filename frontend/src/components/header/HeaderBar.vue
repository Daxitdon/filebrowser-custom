<template>
  <header>
    <!-- <img v-if="showLogo !== undefined" :src="logoURL" />
    <slot /> -->

    <div class="downloadBox">
      <input v-model="downloadLink" placeholder="https://civitai.com/api/download/models/143906" />
      <button @click="downloadFile">Download</button>
    </div>

    <div id="dropdown" :class="{ active: this.$store.state.show === 'more' }">
      <slot name="actions" />
    </div>

    <action v-if="this.$slots.actions" id="more" icon="more_vert" :label="$t('buttons.more')"
      @action="$store.commit('showHover', 'more')" />

    <div class="overlay" v-show="this.$store.state.show == 'more'" @click="$store.commit('closeHovers')" />


  </header>
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
    openSidebar() {
      this.$store.commit("showHover", "sidebar");
    },
    async downloadFile() {
      
      try {
        this.$toast.info('Downloading...');
        await api.downloadFile(this.$route.path, this.downloadLink);
        this.$toast.success('Downloaded successfully!');
        this.$toast.info("Please refresh");
      } catch (error) {
        this.$toast.error('An error occurred while downloading!');
      }
        
    },
  },
};
</script>

<style></style>


