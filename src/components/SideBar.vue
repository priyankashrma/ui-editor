<template>
  <div class="flex flex-col basis-1/4 text-left text-slate-50 bg-gray-950 p-2">
    <div class="text-lg font-bold ml-0.5">Texts</div>
    <div class="mx-1 my-2">
      <SideBarButton
        text="Add a Heading"
        elementType="text"
        classes="text-2xl font-bold bg-gray-800 hover:bg-gray-700"
        elementClasses="text-2xl font-bold"
        foregroundColor="black"
        @pushTextElement="pushTextElement"
      />
      <SideBarButton
        text="Add a subheading"
        elementType="text"
        classes="text-lg font-bold bg-gray-800 hover:bg-gray-700"
        elementClasses="text-lg font-bold"
        foregroundColor="black"
        @pushTextElement="pushTextElement"
      />
      <SideBarButton
        text="Add a little bit of body text"
        elementType="text"
        classes="text-sm bg-gray-800 hover:bg-gray-700"
        elementClasses="text-sm"
        foregroundColor="black"
        @pushTextElement="pushTextElement"
      />
    </div>

    <div class="text-lg font-bold ml-0.5">Elements</div>
    <div class="mx-1 my-2">
      <SideBarButton
        text="Text Input"
        elementType="textinput"
        classes="text-md font-bold bg-slate-100 text-slate-900 hover:bg-slate-300"
        elementClasses="text-md font-bold bg-slate-100 text-slate-900  w-80 h-16 px-2 rounded-lg"
        @pushTextInputElement="pushTextInputElement"
      />
      <div class="flex justify-between">
        <SideBarButton
          text="Button"
          elementType="button"
          classes="text-md font-bold bg-gray-800 hover:bg-gray-700"
          elementClasses="text-lg font-bold bg-gray-800 p-2 mb-2 rounded-lg w-80 h-16 flex justify-center items-center w-full text-slate-100"
          @pushButtonElement="pushButtonElement"
        />
        <SideBarButton
          text=""
          elementType="star"
          imageAsset="stars.svg"
          classes="bg-gray-800 hover:bg-gray-700 ml-1"
          @pushStarElement="pushStarElement"
        />
      </div>
    </div>
    <div class="text-lg font-bold ml-0.5 mb-2">Actions</div>
    <div class="flex justify-between">
      <SideBarButton
        text="Preview"
        classes="font-bold bg-indigo-900 hover:bg-indigo-800"
        @click="previewAction"
      />
      <SideBarButton
        :text="saveButtonText"
        classes="font-bold bg-emerald-900 hover:bg-emerald-800 ml-1"
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
        classes: elementInfo.classes,
        color: elementInfo.color,
        top: `${insertAtTopOffset}px`,
        left: "140px",
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
        classes: elementInfo.classes,
        top: `${insertAtTopOffset}px`,
        left: "140px",
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
        classes: elementInfo.classes,
        top: `${insertAtTopOffset}px`,
        left: "140px",
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
        classes: elementInfo.classes,
        top: `${insertAtTopOffset}px`,
        left: "140px",
      });
      this.lastInsertedElementTopOffset = insertAtTopOffset;
      this.$emit("modifyCurrentSession", tempSession);
    },
    saveAction() {
      this.saveButtonText = "Saved!";
      setTimeout(() => {
        this.saveButtonText = "Save";
      }, 500);
      localStorage.setItem(
        "savedSessionNew",
        JSON.stringify(this.currentSession)
      );
    },
    previewAction() {
      console.log("sss");
      this.$emit("modifyPreviewPopup", true);
    },
  },
};
</script>
