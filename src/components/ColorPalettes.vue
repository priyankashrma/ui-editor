<template>
  <div>
    <div class="colorPalettes">
      <div
        v-for="(item, index) in colors"
        :key="index"
        class="palette"
        :style="{ background: item }"
        @click="handlePaletteClick(item)"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "TopBar",
  props: {
    currentSelectedElement: Object,
  },
  data() {
    return {
      colors: [
        "#fff",
        "#000",
        "#E17A5F",
        "#004AAD",
        "#FD0404",
        "#00BF63",
        "#545454",
        "#5E17EB",
        "#FFDE59",
      ],
    };
  },
  methods: {
    handlePaletteClick(color) {
      this.$emit("handleColorSelection", color);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.palette {
  width: 36px;
  height: 36px;
  margin: 2px;
  border-radius: 4px;
  box-shadow: 0px 0px 5px #ccc;
}
.colorPalettes {
  display: flex;
}
</style>
