<template>
  <div class="pagina">
    <BaseImageViewer
      :selected-image="selectedImageToViewer"
      :images="selectedImageList"
      @close-viewer="closeViewer"
    />
    <div class="mapa">
      <h3>Mapa</h3>
      <div class="modal-direita">
        <h3>Conteudo da modal</h3>
        <ul id="images-blub-teste" class="list-image-test">
          <li
            v-for="(imgItem, index) in imageList"
            :key="index"
            style="display: inline-block"
            @click="selectImageToViewer(index, 'images-blub-teste', imageList)"
          >
            <img :src="imgItem.img" alt="" class="" style="width: 70px" />
          </li>
        </ul>
        <br /><br />
        <ul id="images-blub-blub" class="list-image-test">
          <li
            v-for="(imgItem, index) in imageList2"
            :key="index"
            style="display: inline-block"
            @click="selectImageToViewer(index, 'images-blub-blub', imageList2)"
          >
            <img :src="imgItem.img" alt="" class="" style="width: 70px" />
          </li>
        </ul>
        <br /><br />
        <ul id="images-blub-blub2" class="list-image-test">
          <li
            v-for="(imgItem, index) in imageList3"
            :key="index"
            style="display: inline-block"
            @click="selectImageToViewer(index, 'images-blub-blub2', imageList3)"
          >
            <img :src="imgItem.img" alt="" class="" style="width: 70px" />
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import BaseImageViewer from "./imageViewer/BaseImageViewer";
import {
  ramdomImageList,
  ramdomImageListTeste2,
  ramdomImageListTeste3,
} from "../enums/imagesList";

export default {
  data() {
    return {
      selectedImageToViewer: {
        index: "",
        divImageRef: "",
        images: [],
      },
      currentImage: "", // 15 9 400 70 40
      selectedImageList: [],
    };
  },
  components: {
    BaseImageViewer,
  },
  computed: {
    imageList() {
      return ramdomImageList;
    },
    imageList2() {
      return ramdomImageListTeste2;
    },
    imageList3() {
      return ramdomImageListTeste3;
    },
  },
  methods: {
    selectImageToViewer(imageIndex, imageContainerRef, images) {
      const currentSelectedImageToViewer = this.selectedImageToViewer.index;
      const currentImageIndex = imageIndex.toString();

      if (currentSelectedImageToViewer === currentImageIndex) {
        this.selectedImageToViewer.index = "";
        this.selectedImageToViewer.divImageRef = "";
        this.selectedImageList = [];
      } else {
        this.selectedImageToViewer.index = currentImageIndex;
        this.selectedImageToViewer.divImageRef = imageContainerRef;
        this.selectedImageList = images;
      }
    },
    changeImage(value) {
      let currentValue = value;

      if (this.currentImage === value) currentValue = "";
      this.currentImage = currentValue;
    },
    changeImage2(value) {
      this.$emit("change-image", value);
    },
    closeViewer() {
      this.selectedImageToViewer.index = "";
      this.selectedImageToViewer.divImageRef = "";
      this.selectedImageList = [];
    },
  },
};
</script>

<style lang="scss" scoped>
.pagina {
  position: absolute;
  width: 100%;
  height: 100vh;
  background: burlywood;
}
.mapa {
  background: blueviolet;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
.modal-direita {
  background: yellow;
  position: absolute;
  width: 300px;
  top: 0;
  bottom: 0;
  right: 0;
}
.list-image-test {
  position: relative;
  list-style-type: none;

  li {
    img {
      -moz-user-select: none;
      -webkit-user-select: none;
      -ms-user-select: none;
      user-select: none;
      -webkit-user-drag: none;
      user-drag: none;
      -webkit-touch-callout: none;

      &:hover {
        cursor: pointer;
      }
    }
  }
}
</style>