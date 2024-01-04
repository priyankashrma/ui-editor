<template>
  <div id="app">
    <div class="flex h-full">
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
            color: "#fff",
            top: "127px",
            left: "89px",
            classes: "text-2xl font-bold",
          },
          {
            type: "textinput",
            content: "E-MAIL",
            classes:
              "text-md font-bold bg-slate-100 text-slate-900 w-80 h-16 px-2 rounded-lg",
            color: "#454545",
            top: "241px",
            left: "94px",
          },
          {
            type: "button",
            content: "SIGN UP",
            classes:
              "text-lg font-bold bg-gray-800 p-2 mb-2 rounded-lg w-80 h-16 flex justify-center items-center w-full text-slate-100",
            color: "white",
            top: "322px",
            left: "97px",
          },
          {
            type: "text",
            content: "No credit card required. No Surprises",
            color: "#fff",
            top: "407px",
            left: "108px",
            zIndex: 0,
          },
          {
            type: "star",
            width: "54px",
            height: "54px",
            top: "52px",
            left: "125px",
            zIndex: 0,
          },
          {
            type: "star",
            width: "54px",
            height: "54px",
            top: "16px",
            left: "216px",
            zIndex: 0,
          },
          {
            type: "star",
            width: "54px",
            height: "54px",
            top: "51px",
            left: "310px",
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
      const savedSession = localStorage.getItem("savedSessionNew");
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
<style src="./assets/tailwind.css" />
<style>
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
