<template>
  <div class="previewPopupContainer" @click.self="closePopup">
    <div
      class="previewPopup"
      ref="canvas"
      :style="{
        background: currentSession.canvas.background,
        borderRadius: currentSession.canvas.borderRadius + 'px',
      }"
    >
      <div
        v-for="(item, index) in currentSession.elements"
        :key="index"
        :style="{
          position: 'absolute',
          top: item.top,
          left: item.left,
        }"
        class="element"
      >
        <template v-if="item.type === 'text'">
          <div
            :style="{
              fontSize: item.fontSize,
              fontWeight: item.fontWeight,
              color: item.color,
              width: '400px',
            }"
          >
            {{ item.content }}
          </div>
        </template>
        <template v-else-if="item.type === 'image'">
          <img :src="item.src" alt="Image" />
        </template>
        <template v-else-if="item.type === 'star'">
          <img
            src="../assets/stars.svg"
            alt="Image"
            :style="{
              width: item.width,
              height: item.height,
              pointerEvents: 'none',
            }"
          />
        </template>
        <template v-else-if="item.type === 'button'">
          <button
            :style="{
              fontSize: item.fontSize,
              fontWeight: item.fontWeight,
              color: item.color,
              background: item.backgroundColor,
              width: item.width,
              padding: '10px',
              height: '65px',
              border: '0px',
              borderRadius: '8px',
            }"
          >
            {{ item.content }}
          </button>
        </template>
        <template v-else-if="item.type === 'textinput'">
          <input
            type="text"
            :style="{
              fontSize: item.fontSize,
              fontWeight: item.fontWeight,
              color: item.color,
              background: item.backgroundColor,
              width: item.width,
              paddingLeft: '10px',
              paddingRight: '10px',
              height: '50px',
              border: '0px',
              borderRadius: '8px',
            }"
            :placeholder="item.content"
          />
        </template>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "inputPopup",
  props: {
    label: String,
    value: String,
    index: Number,
    currentSession: Object,
    currentSelectedElement: Object,
  },
  data() {
    return {
      message:
        this.currentSession.elements[this.currentSelectedElement.index]
          ?.content,
    };
  },
  methods: {
    handleChange() {
      console.log(this.currentValue);
      this.$emit("handleChange", this.currentValue);
    },
    closePopup() {
      this.$emit("modifyPreviewPopup", false);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.previewPopupContainer {
  width: 100%;
  height: 100%;
  backdrop-filter: blur(20px);
  z-index: 100;
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
}
.previewPopup {
  width: 500px;
  height: 500px;
  box-shadow: 0px 0px 10px #ccc;
  animation: popup 0.7s;
  position: relative;
}
@keyframes popup {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.4);
  }
  60% {
    transform: scale(1.1);
  }
  70% {
    transform: scale(1.2);
  }
  80% {
    transform: scale(1);
  }
  90% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}
</style>
