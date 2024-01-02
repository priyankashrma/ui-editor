<template>
  <div class="rangeInput">
    <div class="label">{{ label }}</div>
    <input
      type="range"
      :min="min"
      :max="max"
      v-model="currentValue"
      @change="handleChange"
    />
  </div>
</template>
<script>
export default {
  name: "RangeInput",
  props: {
    value: Number,
    min: Number,
    max: Number,
    label: String,
  },
  data() {
    return {
      currentValue: parseInt(this.value),
    };
  },
  methods: {
    handleChange() {
      console.log(this.currentValue);
      this.$emit("handleChange", this.currentValue);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.rangeInput {
  margin: 0px 10px 10px 0px;
}
</style>
