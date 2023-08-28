<template>
  <header>
    <!-- <img v-if="showLogo !== undefined" :src="logoURL" />
    <slot /> -->

    <div>
      <input v-model="downloadLink" placeholder="Paste link here" />
      <button @click="downloadFile">Download</button>
    </div>

    <div id="dropdown" :class="{ active: this.$store.state.show === 'more' }">
      <slot name="actions" />
    </div>

    <action
      v-if="this.$slots.actions"
      id="more"
      icon="more_vert"
      :label="$t('buttons.more')"
      @action="$store.commit('showHover', 'more')"
    />

    <div
      class="overlay"
      v-show="this.$store.state.show == 'more'"
      @click="$store.commit('closeHovers')"
    />

    
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
      
      await api.downloadFile(this.$route.path,this.downloadLink)
        
    },
  },
};
</script>

<style></style>


