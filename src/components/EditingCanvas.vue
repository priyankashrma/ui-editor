<template>
  <div class="editingCanvasContainer" @click.self="unselectOthers">
    <div class="editingCanvasLimitContainer" @click.self="unselectOthers">
      <div
        class="editingCanvas"
        ref="canvas"
        @click="selectCanvas"
        :style="{
          background: currentSession.canvas.background,
          borderRadius: currentSession.canvas.borderRadius + 'px',
        }"
      >
        <div
          v-for="(item, index) in currentSession.elements"
          :key="index"
          :style="{
            color: item.color,
            position: 'absolute',
            top: item.top,
            left: item.left,
            minWidth: '54px',
          }"
          class="element"
          @click.stop="selectElement(index)"
          @mousedown="startDrag(index)"
          ref="draggableElement"
        >
          <div ref="ElementControlButtons" class="elementControlButtons">
            <div class="editButton" @click="editSelectedElement">
              <img src="../assets/pencil.png" class="editButtonImage" />
            </div>
            <div class="removeButton" @click="removeSelectedElement(index)">
              x
            </div>
          </div>

          <template v-if="item.type === 'text'">
            <div
              @click.stop="handleClick(index)"
              @blur="handleOnBlur(index)"
              :class="item.classes"
              :style="{ width: '320px' }"
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
              :class="item.classes"
              :style="{ pointerEvents: 'none' }"
            />
            <div v-if="item.content">{{ item.content }}</div>
          </template>
          <template v-else-if="item.type === 'button'">
            <button @click="handleButtonClick" :class="item.classes">
              {{ item.content }}
            </button>
          </template>
          <template v-else-if="item.type === 'textinput'">
            <input
              type="text"
              :class="item.classes"
              :placeholder="item.content"
            />
          </template>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "EditingCanvas",
  props: {
    msg: String,
    currentSession: Object,
    currentSelectedElement: Object,
  },
  data() {
    return {
      isDragging: false,
      draggedElement: null,
      dragStart: { x: 0, y: 0 },
      timeoutId: null,
      isEditingInline: false,
    };
  },
  methods: {
    selectCanvas() {
      this.unselectOthers();
      this.$refs.canvas.style.border = "2px solid blue";
      const currentSelected = { type: "canvas", index: 0 };
      this.$emit("modifyCurrentSelectedElement", currentSelected);
    },
    handleClick(index) {
      if (!this.timeoutId) {
        this.timeoutId = setTimeout(() => {
          //single click
          clearTimeout(this.timeoutId);
          this.timeoutId = null;
        }, 300);
      } else {
        // double click
        clearTimeout(this.timeoutId);
        this.timeoutId = null;
        this.handleDoubleClick(index);
      }
    },
    handleDoubleClick(index) {
      const element =
        this.$refs.draggableElement[index].getElementsByTagName("div")[3];
      element.contentEditable = true;
      element.focus();
      this.isEditingInline = true;
    },
    handleOnBlur(index) {
      const element =
        this.$refs.draggableElement[index].getElementsByTagName("div")[3];
      element.contentEditable = false;
      this.isEditingInline = false;
      //saving the changes
      const newText = element.innerText;
      const updatedSession = this.currentSession;
      updatedSession.elements[index].content = newText;
      this.$emit("modifyCurrentSession", updatedSession);
    },
    selectElement(index) {
      this.unselectOthers();
      this.$refs.draggableElement[index].style.border = "2px solid blue";
      this.$refs.ElementControlButtons[index].style.display = "initial";
      const currentSelected = { type: "element", index: index };
      this.$emit("modifyCurrentSelectedElement", currentSelected);
    },
    unselectOthers() {
      this.$refs.draggableElement.map((item) => {
        item.style.border = "0px";
      });
      this.$refs.ElementControlButtons.map((item) => {
        item.style.display = "none";
      });
      this.$refs.canvas.style.border = "0px";
      const currentSelected = { type: "", index: 0 };
      this.$emit("modifyCurrentSelectedElement", currentSelected);
    },
    removeSelectedElement(index) {
      const updatedSession = this.currentSession;
      updatedSession.elements.splice(index, 1);
      this.$emit("modifyCurrentSession", updatedSession);
    },
    editSelectedElement() {
      this.$emit("modifyInputPopup", true);
    },
    handleButtonClick() {
      // Add logic for button click if needed
      console.log("Button clicked!");
    },
    startDrag(index) {
      this.selectElement(index);
      this.isDragging = true;
      this.draggedElement = this.$refs.draggableElement[index];
      this.dragStart = { x: event.clientX, y: event.clientY };

      document.addEventListener("mousemove", this.handleDrag);
      document.addEventListener("mouseup", this.stopDrag);
    },
    handleDrag(event) {
      if (this.isDragging && this.draggedElement && !this.isEditingInline) {
        event.preventDefault();
        const offsetX = event.clientX - this.dragStart.x;
        const offsetY = event.clientY - this.dragStart.y;

        const currentLeft = this.draggedElement.offsetLeft + offsetX;
        const currentTop = this.draggedElement.offsetTop + offsetY;

        const parentWidth = 550;
        const parentHeight = 550;

        // Ensure the element stays within the bounds
        const maxX = parentWidth - this.draggedElement.offsetWidth;
        const maxY = parentHeight - this.draggedElement.offsetHeight;

        const newLeft = Math.max(-50, Math.min(currentLeft, maxX));
        const newTop = Math.max(-50, Math.min(currentTop, maxY));

        this.draggedElement.style.left = newLeft + "px";
        this.draggedElement.style.top = newTop + "px";
        this.dragStart = { x: event.clientX, y: event.clientY };
        const updatedSession = this.currentSession;
        updatedSession.elements[
          this.currentSelectedElement.index
        ].top = `${newTop}px`;

        updatedSession.elements[
          this.currentSelectedElement.index
        ].left = `${newLeft}px`;

        this.$emit("modifyCurrentSession", updatedSession);
      }
    },
    stopDrag() {
      if (this.isDragging) {
        this.isDragging = false;
        this.draggedElement = null;

        document.removeEventListener("mousemove", this.handleDrag);
        document.removeEventListener("mouseup", this.stopDrag);
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.editingCanvasContainer {
  display: flex;
  background: #efefef;
  height: 100%;
  justify-content: center;
  align-items: center;
}
.editingCanvasLimitContainer {
  border: 3px dashed #dfdfdf;
  width: 575px;
  height: 575px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.editingCanvas {
  background: white;
  width: 500px;
  height: 500px;
  position: relative;
}

h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
.element {
  padding: 0px;
}
.element:hover {
  border: 2px solid rgb(71, 71, 232);
  border-radius: 2px;
  cursor: pointer;
}
.editButton {
  color: white;
  background: #43088c;
  width: 28px;
  height: 28px;
  font-weight: bold;
  font-size: 14px;
  border-radius: 20px;
  position: absolute;
  top: -15px;
  right: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.editButtonImage {
  width: 13px;
  height: 13px;
}
.removeButton {
  color: white;
  background: #880808;
  width: 28px;
  height: 28px;
  font-weight: bold;
  font-size: 18px;
  border-radius: 20px;
  position: absolute;
  top: -15px;
  right: -15px;
}
.elementControlButtons {
  display: none;
}
</style>
