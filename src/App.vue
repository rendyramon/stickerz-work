<template>
  <el-container class="container">
    <el-header class="header">
      <h1>stickerz.work</h1>
    </el-header>

    <el-main class="main">
      <el-form :model="stickerPack" class="demo-form-inline" label-position="top">
        <el-row>
          <el-col :span="18">
            <el-form-item label="識別代號">
              <el-input
                v-model="stickerPack.identifier"
                placeholder="(e.g. awesome-001)"
                prefix-icon="el-icon-tickets"
              ></el-input>
            </el-form-item>
            <el-form-item label="貼圖包名稱">
              <el-input
                v-model="stickerPack.name"
                placeholder="(e.g. Awesome Stickers #001)"
                prefix-icon="el-icon-goods"
              ></el-input>
            </el-form-item>
            <el-form-item label="作者">
              <el-input
                v-model="stickerPack.publisher"
                placeholder="(e.g. Awesome Guy)"
                prefix-icon="el-icon-edit-outline"
              ></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="貼圖包圖示" class="right-col">
              <el-upload
                accept="image/*"
                class="avatar-uploader"
                action
                :show-file-list="false"
                :auto-upload="false"
                :on-change="handleTrayPreview"
              >
                <div v-if="tempTrayImage" class="full-width">
                  <img :src="tempTrayImage" class="avatar">
                </div>
                <div v-else class="full-width">
                  <i class="el-icon-plus avatar-uploader-icon"></i>
                </div>
              </el-upload>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-form-item label="貼圖 (3-30張)">
            <el-upload
              accept="image/*"
              action
              :auto-upload="false"
              :on-change="handleStickers"
              list-type="picture-card"
              :limit="30"
              multiple
            >
              <i class="el-icon-plus"></i>
            </el-upload>
          </el-form-item>
          <el-form-item>
            <el-button :disabled="!isComplete" type="primary" style="width: 100%;">生成JSON檔案</el-button>
          </el-form-item>
        </el-row>
      </el-form>
    </el-main>
  </el-container>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      stickers: [],
      tempTrayImage: "",
      stickerPack: {
        name: "",
        publisher: "",
        identifier: "",
        tray_image: "",
        stickers: []
      }
    };
  },
  methods: {
    crop(img, size) {
      let canvas = document.createElement("canvas");
      let ctx = canvas.getContext("2d");
      canvas.height = size;
      canvas.width = size;
      let scale = Math.min(size / img.width, size / img.height);
      let x = size / 2 - (img.width / 2) * scale;
      let y = size / 2 - (img.height / 2) * scale;
      ctx.drawImage(img, x, y, img.width * scale, img.height * scale);
      return canvas;
    },
    handleTrayPreview(file) {
      let reader = new FileReader();
      reader.readAsDataURL(file.raw);
      reader.onloadend = () => {
        let img = new Image();
        img.src = reader.result;
        img.onload = () => {
          this.tempTrayImage = this.crop(img, 512).toDataURL(
            "image/webp",
            0.75
          );
          this.stickerPack.tray_image = this.crop(img, 96)
            .toDataURL("image/png")
            .replace("data:image/png;base64,", "");
        };
      };
    },
    handleStickers(file, fileList) {
      this.stickers = fileList;
    }
  },
  computed: {
    isComplete() {
      return (
        this.stickers.length >= 3 &&
        this.stickers.length <= 30 &&
        this.stickerPack.name &&
        this.stickerPack.publisher &&
        this.stickerPack.identifier &&
        this.trayImage != null
      );
    }
  }
};
</script>

<style scoped>
.right-col {
  margin-left: 2rem;
  padding: 0px;
}
.avatar-uploader {
  border: 1px dashed #c4c4c4;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  height: 0;
  width: 100%;
  padding-bottom: 100%;
}
.avatar-uploader:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  margin: 0;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-right: -50%;
  transform: translate(-50%, -50%);
  line-height: 1000%;
  width: 100%;
}
.avatar {
  width: 100%;
  display: block;
}
.full-width {
  width: auto;
  padding-bottom: 100%;
}
</style>
