<template>
  <button class="button" :style="dynamicStyle" @click="handleButtonClick">
    <img :src="resolveImgUrl(image)" v-if="imageAsset" class="image" />
    {{ text }}
  </button>
</template>

<script>
export default {
  name: "SideBarButton",
  props: {
    text: String,
    fontSize: String,
    fontWeight: String,
    bgColor: String,
    foregroundColor: String,
    textAlign: String,
    imageAsset: String,
    marginLeft: String,
    elementType: String,
  },

  computed: {
    dynamicStyle() {
      return {
        fontSize: this.fontSize,
        fontWeight: this.fontWeight,
        backgroundColor: this.bgColor ? this.bgColor : "#434343",
        color: this.foregroundColor ? this.foregroundColor : "white",
        textAlign: this.textAlign ? this.textAlign : "left",
        marginLeft: this.marginLeft ? this.marginLeft : 0,
      };
    },
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
          fontSize: this.fontSize,
          fontWeight: this.fontWeight,
          content: this.text,
        });
      } else if (this.elementType === "button") {
        this.$emit("pushButtonElement", {
          fontSize: this.fontSize,
          fontWeight: this.fontWeight,
          content: this.text,
          backgroundColor: this.bgColor ? this.bgColor : "#434343",
          width: "352px",
        });
      } else if (this.elementType === "textinput") {
        this.$emit("pushTextInputElement", {
          fontSize: this.fontSize,
          fontWeight: this.fontWeight,
          content: this.text,
          backgroundColor: this.bgColor ? this.bgColor : "#434343",
          color: this.foregroundColor ? this.foregroundColor : "white",
          width: "332px",
        });
      } else if (this.elementType === "star") {
        this.$emit("pushStarElement", {
          width: "54px",
          height: "54px",
        });
      } else this.$emit("click");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.button {
  width: 100%;
  height: 50px;
  background: #434343;
  color: white;
  border: 0px;
  cursor: pointer;
  margin-top: 10px;
  text-align: left;
  padding: 10px;
  border-radius: 5px;
  font-size: var(--font-size);
  transition: 0.2s;
  display: flex;
  justify-content: center;
  align-items: center;
}

.image {
  height: 30px;
  width: 30px;
  margin-right: 10px;
}

.button:hover {
  transform: scale(0.96);
}
</style>
