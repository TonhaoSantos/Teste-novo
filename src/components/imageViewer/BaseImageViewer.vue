<template>
  <div v-if="isValid">
    <BaseImageViewerZoomButton
      @change-zoom="changeZoom"
      class="base-image-tools-itens"
    />
    <br />
    <BaseImageViewerCloseButton @close-viewer="fechar" />
    <br />
    <BaseImageViewerSlider
      :images="images"
      @change-image="changeImage"
      @change-image-prev-next="changeImagePrevNext"
    />
    <div class="base-image-tools"></div>
  </div>
</template>

<script>
import BaseImageViewerCloseButton from "./BaseImageViewerCloseButton";
import BaseImageViewerSlider from "./BaseImageViewerSlider";
import BaseImageViewerZoomButton from "./BaseImageViewerZoomButton";

import "viewerjs/dist/viewer.css";
import Viewer from "viewerjs";

export default {
  name: "BaseImageViewerImage",
  props: {
    selectedImage: {
      type: Object,
      required: true,
      validator: function (value) {
        const validObj = ["index", "divImageRef"];

        return validObj.every((item) => value.hasOwnProperty(item));
      },
    },
    images: {
      type: Array,
      required: true,
    },
  },
  components: {
    BaseImageViewerCloseButton,
    BaseImageViewerSlider,
    BaseImageViewerZoomButton,
  },
  data() {
    return {
      activeImage: {},
      showModal: false,
      viewer: "",
      zoom: {
        number: 0,
        type: "",
      },
      config: {
        button: false,
        loading: true,
        navbar: false,
        toggleOnDblclick: true,
        zoomable: true,
        zoomOnTouch: true,
        zoomOnWheel: false,
        inline: false,
        backdrop: false,
        focus: false,
        fullscreen: false,
        loop: false,
        keyboard: false,
        movable: true,
        rotatable: false,
        scalable: false,
        slideOnTouch: false,
        title: false,
        toolbar: false,
        tooltip: true,
        transition: false,
        minZoomRatio: 0.9,
        maxZoomRatio: 10,
      },
    };
  },
  computed: {
    isValid() {
      const imgObj = this.selectedImage;
      const notEmptyObj = this.isEmptyObj(imgObj);

      return !!notEmptyObj;
    },
    activeImageUrl() {
      return this.activeImage.img || "";
    },
  },
  methods: {
    isEmptyObj(imgObj) {
      const noIsEmpty = !!(
        Object.keys(imgObj).length !== 0 && imgObj.constructor === Object
      );
      const hasValues = !!(imgObj.index && imgObj.divImageRef);

      return !!(noIsEmpty && hasValues);
    },
    getImage(imageIndexValue) {
      const img = this.images.filter(
        (item, index) => index === imageIndexValue
      )[0];

      this.activeImage = img;

      this.selectImage();
    },
    mountViewer() {
      const imageIndex = this.selectedImage.index;
      const imageRef = document.getElementById(
        `${this.selectedImage.divImageRef}`
      );

      console.log("imageRef -> ", imageRef);

      if (imageRef) {
        const viewerConfig = this.config;
        const _ = this;
        this.viewer = new Viewer(imageRef, {
          ...viewerConfig,
          ready() {
            _.changeImage(imageIndex);
          },
        });
      }
    },
    selectImage() {
      this.mountViewer();
      this.disableZoomLessButton = true;
      this.showModal = true;
    },
    changeImage(imageIndex) {
      this.viewer.view(imageIndex);
    },
    changeZoom(value) {
      this.zoom = value;
    },
    fechar() {
      this.viewer.destroy();
      this.disableZoomLessButton = false;
      this.showModal = false;

      this.$emit("close-viewer");
    },
    changeImagePrevNext(type) {
      if (type === "prev") {
        this.viewer.prev();
      } else {
        this.viewer.next();
      }
    },
  },
  watch: {
    selectedImage: {
      deep: true,
      immediate: true,
      handler(imageIndexValue) {
        console.log(
          "this.isEmptyObj(imageIndexValue) ",
          this.isEmptyObj(imageIndexValue)
        );
        if (this.isValid) this.getImage(imageIndexValue);
      },
    },
    zoom: {
      deep: true,
      immediate: true,
      handler(value) {
        if (value.type) {
          console.log("watch zoomIn -> ", value);
          let ratio = 0.1;

          if (value.type !== "plus") {
            ratio = -0.1;
          }

          console.log("--> ", ratio);
          this.viewer.zoom(ratio);
        }
      },
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.base-image-viewer {
  position: relative;
  list-style-type: none;
}
.viewer-container {
  z-index: 90001 !important;
  display: inline-block !important;
}
.base-image-tools {
  position: absolute !important;
  display: block;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 90001;
  background: black;
}
.base-image-tools-itens {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 90002;
}
</style>








