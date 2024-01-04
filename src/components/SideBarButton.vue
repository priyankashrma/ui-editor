<template>
  <button
    :class="`${classes} p-2 mb-2 rounded-lg w-full flex justify-center items-center`"
    @click="handleButtonClick"
  >
    <img :src="resolveImgUrl(image)" v-if="imageAsset" class="w-8 h-8" />
    {{ text }}
  </button>
</template>

<script>
export default {
  name: "SideBarButton",
  props: {
    text: String,
    imageAsset: String,
    elementType: String,
    elementClasses: String,
    classes: String,
    foregroundColor: String,
    bgColor: String,
  },

  computed: {
    imageSrc() {
      return new URL(this.image, import.meta.url);
    },
  },
  methods: {
    resolveImgUrl() {
      const images = require.context("../assets/", false, /\.svg$/);
      return images(`./${this.imageAsset}`);
    },
    handleButtonClick() {
      if (this.elementType === "text") {
        this.$emit("pushTextElement", {
          classes: this.elementClasses,
          content: this.text,
          color: this.foregroundColor,
        });
      } else if (this.elementType === "button") {
        this.$emit("pushButtonElement", {
          classes: this.elementClasses,
          content: this.text,
        });
      } else if (this.elementType === "textinput") {
        this.$emit("pushTextInputElement", {
          classes: this.elementClasses,
          content: "Placeholder...",
        });
      } else if (this.elementType === "star") {
        this.$emit("pushStarElement", {
          classes: this.elementClasses,
        });
      } else this.$emit("click");
    },
  },
};
</script>
