<template>
  <div id="app">
    <div class="parentContainer">
      <InputPopup
        v-if="inputPopup"
        :currentSession="currentSession"
        :currentSelectedElement="currentSelectedElement"
        @modifyInputPopup="modifyInputPopup"
        @modifyCurrentSession="modifyCurrentSession"
      />
      <PreviewPopup
        v-if="previewPopup"
        :currentSession="currentSession"
        :currentSelectedElement="currentSelectedElement"
        @modifyPreviewPopup="modifyPreviewPopup"
      />
      <SideBar
        :currentSession="currentSession"
        @modifyCurrentSession="modifyCurrentSession"
        @modifyPreviewPopup="modifyPreviewPopup"
      />
      <div class="rightSideContainer">
        <TopBar
          :currentSelectedElement="currentSelectedElement"
          :currentSession="currentSession"
          @modifyCurrentSession="modifyCurrentSession"
        />
        <EditingCanvas
          :currentSession="currentSession"
          :currentSelectedElement="currentSelectedElement"
          :inputPopup="inputPopup"
          @modifyCurrentSession="modifyCurrentSession"
          @modifyCurrentSelectedElement="modifyCurrentSelectedElement"
          @modifyInputPopup="modifyInputPopup"
        />
      </div>
    </div>
  </div>
</template>

<script>
import SideBar from "./components/SideBar.vue";
import TopBar from "./components/TopBar.vue";
import EditingCanvas from "./components/EditingCanvas.vue";
import InputPopup from "./components/InputPopup.vue";
import PreviewPopup from "./components/PreviewPopup.vue";
export default {
  name: "App",
  components: {
    SideBar,
    TopBar,
    EditingCanvas,
    InputPopup,
    PreviewPopup,
  },
  data() {
    return {
      currentSession: {
        canvas: {
          background: "#E17A5F",
          borderRadius: "400",
        },
        elements: [
          {
            type: "text",
            content:
              "All the text and elements in this popup should be editable and dragable",
            fontSize: "24px",
            fontWeight: "bold",
            color: "#fff",
            top: "164px",
            left: "53px",
            zIndex: 0,
          },
          {
            type: "textinput",
            content: "E-MAIL",
            fontSize: "18px",
            backgroundColor: "white",
            width: "332px",
            color: "#454545",
            top: "279px",
            left: "78px",
            zIndex: 0,
          },
          {
            type: "button",
            content: "SIGN UP",
            fontSize: "18px",
            fontWeight: "bold",
            backgroundColor: "#434343",
            width: "352px",
            color: "white",
            top: "339px",
            left: "80px",
            zIndex: 0,
          },
          {
            type: "text",
            content: "No credit card required. No Surprises",
            fontSize: "18px",
            fontWeight: "bold",
            color: "#fff",
            top: "415px",
            left: "57px",
            zIndex: 0,
          },
          {
            type: "star",
            width: "54px",
            height: "54px",
            top: "83px",
            left: "134px",
            zIndex: 0,
          },
          {
            type: "star",
            width: "54px",
            height: "54px",
            top: "49px",
            left: "220px",
            zIndex: 0,
          },
          {
            type: "star",
            width: "54px",
            height: "54px",
            top: "81px",
            left: "315px",
            zIndex: 0,
          },
        ],
      },
      currentSelectedElement: { type: "", index: 0 },
      inputPopup: false,
      previewPopup: false,
    };
  },
  methods: {
    modifyCurrentSession(sessionData) {
      this.currentSession = sessionData;
    },
    modifyCurrentSelectedElement(currentSelected) {
      this.currentSelectedElement = currentSelected;
    },
    modifyInputPopup(value) {
      this.inputPopup = value;
    },
    modifyPreviewPopup(value) {
      this.previewPopup = value;
    },
    getSession() {
      const savedSession = localStorage.getItem("savedSession");
      if (savedSession) {
        this.currentSession = JSON.parse(savedSession);
      }
    },
  },
  created() {
    this.getSession();
  },
};
</script>

<style>
.parentContainer {
  display: flex;
  flex-direction: row;
  height: 100%;
}

.rightSideContainer {
  flex: 1;
  display: flex;
  flex-direction: column;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 0px;
  height: 100%;
}

body {
  margin: 0px;
  height: 100%;
}

html {
  height: 100%;
}
</style>
