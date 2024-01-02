<template>
  <div class="topbar">
    <ColorPalettes
      v-if="currentSelectedElement?.type"
      @handleColorSelection="handleColorSelection"
    />
    <div class="rangeInputs" v-if="currentSelectedElement?.type">
      <RangeInput
        :min="0"
        :max="400"
        :value="currentSession.canvas.borderRadius"
        label="Corner Rounding"
        v-if="currentSelectedElement?.type === 'canvas'"
        @handleChange="handleCornerRoundingChange"
      />
    </div>
  </div>
</template>
<script>
import ColorPalettes from "./ColorPalettes.vue";
import RangeInput from "./RangeInput.vue";
export default {
  name: "TopBar",
  props: {
    currentSelectedElement: Object,
    currentSession: Object,
  },
  components: {
    ColorPalettes,
    RangeInput,
  },
  methods: {
    handleColorSelection(color) {
      const tempSession = this.currentSession;
      if (this.currentSelectedElement.type === "canvas") {
        tempSession.canvas.background = color;
      } else {
        tempSession.elements[this.currentSelectedElement.index].color = color;
      }

      this.$emit("modifyCurrentSession", tempSession);
    },
    handleCornerRoundingChange(value) {
      const tempSession = this.currentSession;
      tempSession.canvas.borderRadius = value;
      this.$emit("modifyCurrentSession", tempSession);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.topbar {
  width: 100%;
  height: 50px;
  background: #fff;
  display: flex;
  justify-content: flex-start;
}
.rangeInputs {
  margin-left: 20px;
}
</style>
