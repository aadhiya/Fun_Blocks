<script lang="ts">
import { defineComponent } from 'vue-demi';

export default defineComponent({
  data() {
     return {
      currentEvent: {},
      editableText: "I am editable text. Drag this box around and edit me!", // Initial text
    };
  },
  methods: {
    onStart(e) {
      this.currentEvent = e;
    },
    onStop() {
      this.currentEvent = {};
    },
    onMove(e) {
      this.currentEvent = e;
    },
    onTextInput(event) {
      this.editableText = event.target.value; // Update the text as the user types
    },
  },
});
</script>

<template>
  <div id="demo">
     <!-- Main draggable element -->
    <div v-draggable="{ handle: 'strong' }" class="draggable-box">
      <div style="display: flex; flex-direction: column; max-height: 140px; padding: 10px;">
        <strong class="cursor"> Drag here </strong>
        <div style="overflow: scroll; margin-top: 10px;">
          <textarea
            v-model="editableText"
            class="editable-textbox"
            rows="5"
            style="width: 100%; resize: none;"
          ></textarea>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* Minimal required styles */
#demo {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f0f0f0;
}

.cursor {
  cursor: grab;
}

.draggable-box {
  border: 2px solid #6b7280;
  border-radius: 8px;
  background-color: #ffffff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 300px;
}

.editable-textbox {
  font-size: 16px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
  background-color: #f9f9f9;
  color: #333;
}
.editable-textbox:focus {
  border-color: #4a90e2;
  background-color: #fff;
}
</style>
