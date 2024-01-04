<template>
  <div class="editingCanvasContainer" @click.self="unselectOthers">
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
          position: 'absolute',
          top: item.top,
          left: item.left,
        }"
        class="element"
        @click.stop="selectElement(index)"
        @mousedown.prevent="startDrag(index)"
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
          <div v-if="item.content">{{ item.content }}</div>
        </template>
        <template v-else-if="item.type === 'button'">
          <button
            @click="handleButtonClick"
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
  name: "EditingCanvas",
  props: {
    msg: String,
    currentSession: Object,
    currentSelectedElement: Object,
  },
  data() {
    return {
      testPre: {
        canvas: {
          background: "#fff",
          borderRadius: 0,
        },
        elements: [
          {
            type: "text",
            content: "hello world",
            fontSize: "24px",
            fontWeight: "bold",
            color: "black",
            top: 20,
            left: 30,
            zIndex: 0,
          },
          {
            type: "text",
            content: "this is the test",
            fontSize: "18px",
            fontWeight: "bold",
            color: "red",
            top: 50,
            left: 30,
            zIndex: 1,
          },
        ],
      },
      isDragging: false,
      draggedElement: null,
      dragStart: { x: 0, y: 0 },
    };
  },
  methods: {
    selectCanvas() {
      this.unselectOthers();
      this.$refs.canvas.style.border = "2px solid blue";
      const currentSelected = { type: "canvas", index: 0 };
      this.$emit("modifyCurrentSelectedElement", currentSelected);
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
      if (this.isDragging && this.draggedElement) {
        const offsetX = event.clientX - this.dragStart.x;
        const offsetY = event.clientY - this.dragStart.y;
        const newLeft = this.draggedElement.offsetLeft + offsetX;
        const newTop = this.draggedElement.offsetTop + offsetY;
        if (newLeft + this.draggedElement.offsetWidth > 550 || newLeft < -50) {
          this.stopDrag();
          return;
        }
        if (newTop + this.draggedElement.offsetHeight > 550 || newTop < -50) {
          this.stopDrag();
          return;
        }
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
