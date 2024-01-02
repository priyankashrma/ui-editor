<template>
  <div class="sidebar">
    <div class="sidebarHeading">Texts</div>
    <div class="buttonContainer">
      <SideBarButton
        text="Add a Heading"
        font-size="24px"
        font-weight="bold"
        elementType="text"
        @pushTextElement="pushTextElement"
      />
      <SideBarButton
        text="Add a subheading"
        font-size="18px"
        font-weight="bold"
        elementType="text"
        @pushTextElement="pushTextElement"
      />
      <SideBarButton
        text="Add a little bit of body text"
        font-size="13px"
        elementType="text"
        @pushTextElement="pushTextElement"
      />
    </div>

    <div class="sidebarHeading">Elements</div>
    <div class="buttonContainer">
      <SideBarButton
        text="Text Input"
        font-size="18px"
        bgColor="white"
        foregroundColor="#454545"
        elementType="textinput"
        @pushTextInputElement="pushTextInputElement"
      />
      <div class="twoButtonContainer">
        <SideBarButton
          text="Button"
          font-size="18px"
          font-weight="bold"
          text-align="center"
          elementType="button"
          @pushButtonElement="pushButtonElement"
        />
        <SideBarButton
          text=""
          font-size="18px"
          font-weight="bold"
          imageAsset="stars.svg"
          text-align="center"
          margin-left="5px"
          elementType="star"
          @pushStarElement="pushStarElement"
        />
      </div>
    </div>
    <div class="sidebarHeading">Actions</div>
    <div class="twoButtonContainer">
      <SideBarButton
        text="Preview"
        font-size="18px"
        font-weight="bold"
        text-align="center"
        bgColor="purple"
        @click="previewAction"
      />
      <SideBarButton
        :text="saveButtonText"
        font-size="18px"
        font-weight="bold"
        text-align="center"
        margin-left="5px"
        bgColor="green"
        @click="saveAction"
      />
    </div>
  </div>
</template>

<script>
import SideBarButton from "./SideBarButton.vue";
export default {
  name: "SideBar",
  props: {
    msg: String,
    currentSession: Object,
  },
  components: {
    SideBarButton,
  },
  data() {
    return {
      lastInsertedElementTopOffset: 0,
      saveButtonText: "Save",
    };
  },
  methods: {
    pushTextElement(elementInfo) {
      const tempSession = this.currentSession;
      const insertAtTopOffset =
        parseInt(this.lastInsertedElementTopOffset) + 30;
      tempSession.elements.push({
        type: "text",
        content: elementInfo.content,
        fontSize: elementInfo.fontSize,
        fontWeight: elementInfo.fontWeight,
        color: "black",
        top: `${insertAtTopOffset}px`,
        left: "140px",
        zIndex: 0,
      });
      this.lastInsertedElementTopOffset = insertAtTopOffset;
      this.$emit("modifyCurrentSession", tempSession);
    },
    pushButtonElement(elementInfo) {
      const tempSession = this.currentSession;
      const insertAtTopOffset =
        parseInt(this.lastInsertedElementTopOffset) + 30;
      tempSession.elements.push({
        type: "button",
        content: elementInfo.content,
        fontSize: elementInfo.fontSize,
        fontWeight: elementInfo.fontWeight,
        backgroundColor: elementInfo.backgroundColor,
        width: elementInfo.width,
        color: "white",
        top: `${insertAtTopOffset}px`,
        left: "140px",
        zIndex: 0,
      });
      this.lastInsertedElementTopOffset = insertAtTopOffset;
      this.$emit("modifyCurrentSession", tempSession);
    },
    pushTextInputElement(elementInfo) {
      const tempSession = this.currentSession;
      const insertAtTopOffset =
        parseInt(this.lastInsertedElementTopOffset) + 30;
      tempSession.elements.push({
        type: "textinput",
        content: elementInfo.content,
        fontSize: elementInfo.fontSize,
        fontWeight: elementInfo.fontWeight,
        backgroundColor: elementInfo.backgroundColor,
        width: elementInfo.width,
        color: elementInfo.color,
        top: `${insertAtTopOffset}px`,
        left: "140px",
        zIndex: 0,
      });
      this.lastInsertedElementTopOffset = insertAtTopOffset;
      this.$emit("modifyCurrentSession", tempSession);
    },
    pushStarElement(elementInfo) {
      const tempSession = this.currentSession;
      const insertAtTopOffset =
        parseInt(this.lastInsertedElementTopOffset) + 30;
      tempSession.elements.push({
        type: "star",
        width: elementInfo.width,
        height: elementInfo.height,
        top: `${insertAtTopOffset}px`,
        left: "140px",
        zIndex: 0,
      });
      this.lastInsertedElementTopOffset = insertAtTopOffset;
      this.$emit("modifyCurrentSession", tempSession);
    },
    saveAction() {
      this.saveButtonText = "Saved!";
      setTimeout(() => {
        this.saveButtonText = "Save";
      }, 500);
      localStorage.setItem("savedSession", JSON.stringify(this.currentSession));
    },
    previewAction() {
      console.log("sss");
      this.$emit("modifyPreviewPopup", true);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.sidebar {
  display: flex;
  flex-direction: column;
  flex: 0.3;
  height: 100%;
  background: #2e2e2f;
  color: #efefef;
  padding: 10px;
  text-align: left;
}

.sidebarHeading {
  font-size: 16px;
  font-weight: bold;
}

.buttonContainer {
  margin: 10px 0px 40px 0px;
}

.twoButtonContainer {
  display: flex;
  justify-content: space-between;
}
</style>
