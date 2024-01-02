<template>
  <div class="inputPopupContainer" @click.self="closePopup">
    <div class="inputPopup">
      <div class="label">{{ label }}</div>
      <br />
      <textarea
        v-model="message"
        class="textInput"
        rows="10"
        placeholder="enter text....."
      ></textarea>

      <Button class="saveButton" @click="handleSave">Save</Button>
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
      this.$emit("modifyInputPopup", false);
    },
    handleSave() {
      // Update the original array and emit the update to the parent
      const updatedSession = this.currentSession;
      updatedSession.elements[this.currentSelectedElement.index].content =
        this.message;
      this.$emit("modifyCurrentSession", updatedSession);
      this.$emit("modifyInputPopup", false);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.inputPopupContainer {
  width: 100%;
  height: 100%;
  backdrop-filter: blur(20px);
  z-index: 100;
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
}
.inputPopup {
  width: 400px;
  height: 400px;
  background: white;
  box-shadow: 0px 0px 10px #ccc;
  border-radius: 8px;
  animation: popup 0.7s;
  padding-top: 20px;
}
.textInput {
  border: 1px solid #efefef;
  width: 80%;
  padding: 5px;
  border-radius: 10px;
  font-size: 18px;
}
.saveButton {
  width: 80%;
  background: #303030;
  color: white;
  padding: 10px;
  font-size: 18px;
  font-weight: bold;
  border-radius: 10px;
  border: 0px;
  margin-top: 20px;
  cursor: pointer;
}
.saveButton:hover {
  background: #535353;
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
