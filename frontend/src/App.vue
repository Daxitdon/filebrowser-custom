<template>
  <div>
    <router-view></router-view>
  </div>
</template>

<script>
// eslint-disable-next-line no-undef
__webpack_public_path__ = window.FileBrowser.StaticURL + "/";

export default {
  name: "app",
  mounted() {
    const loading = document.getElementById("loading");
    loading.classList.add("done");

    setTimeout(function () {
      loading.parentNode.removeChild(loading);
    }, 200);

    // Add event listeners for URL changes
    window.addEventListener('load', this.onUrlChange);
    window.addEventListener('hashchange', this.onUrlChange);
  },
  methods: {
    onUrlChange() {
      // Send URL to parent window
      window.parent.postMessage(window.location.href, '*'); 
    }
  }
};
</script>

<style>
@import "./css/styles.css";
</style>
