<template>
  <el-container class="container">
    <el-header class="header">
      <h1>stickerz.work</h1>
      <p>WhatsApp Sticker Pack JSON Generator</p>
    </el-header>
    <el-main class="main">
      <p>主要</p>
    </el-main>
    <el-footer class="footer">
      <p>註腳</p>
    </el-footer>
  </el-container>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      stickerPack: {
        name: "",
        publisher: "",
        identifier: "",
        tray_image: "",
        stickers: []
      }
    };
  }
};
</script>

<style scoped>
.header {
  font-family: "Nunito", sans-serif;
}
</style>
