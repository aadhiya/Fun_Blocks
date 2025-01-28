<script lang="ts">
import { defineComponent } from 'vue-demi';
// Import images from the assets folder
import image1 from './assets/image1.jpg';
import image2 from './assets/image2.jpg';
import image3 from './assets/image3.jpg';

export default defineComponent({
  data() {
    return {
       blocks: [
        {
          id: 1,
          type: 'text',
          editableText: 'I am editable text. Drag this box around and edit me!',
        },
        {
          id: 2,
          type: 'image',
          selectedImage: image1,
        },
      ],
      predefinedImages: [
        { label: 'Image 1', value: image1 },
        { label: 'Image 2', value: image2 },
        { label: 'Image 3', value: image3 },
      ],
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
   onImageChange(event, blockId) {
      const block = this.blocks.find((b) => b.id === blockId);
      if (block) block.selectedImage = event.target.value;
    },
    duplicateBlock(block) {
      const newBlock = { ...block, id: this.blocks.length + 1 };
      // Ensure a unique ID for each duplicated block
      if (block.type === 'text') {
        newBlock.editableText = `${block.editableText} (Copy)`;
      } else if (block.type === 'image') {
        newBlock.selectedImage = block.selectedImage;
      }
      this.blocks.push(newBlock);
    },
    deleteBlock(blockId) {
      this.blocks = this.blocks.filter((block) => block.id !== blockId);
    },
  },
});
</script>

<template>
  <div id="demo">
    <!-- Iterate over blocks to display them -->
    <div
      v-for="block in blocks"
      :key="block.id"
      v-draggable="{ handle: 'strong' }"
      class="draggable-box"
      style="margin-bottom: 10px"
    >
      <!-- Text Block -->
      <div
        v-if="block.type === 'text'"
        style="padding: 10px; display: flex; flex-direction: column; max-height: 140px;"
      >
        <strong class="cursor">Drag here</strong>
        <div style="overflow: scroll; margin-top: 10px;">
          <textarea
            v-model="block.editableText"
            class="editable-textbox"
            rows="5"
            style="width: 100%; resize: none; padding: 10px; border: 1px solid #ccc; border-radius: 4px;"
          ></textarea>
        </div>
        <button @click="duplicateBlock(block)">Duplicate</button>
        <button @click="deleteBlock(block.id)">Delete</button>
      </div>

      <!-- Image Block -->
      <div
        v-if="block.type === 'image'"
        style="display: flex; flex-direction: column; padding: 10px; overflow: visible; min-height: 300px;"
      >
        <strong class="cursor">Drag here</strong>
        <div style="margin-top: 10px;">
          <label for="imageSelector" style="display: block; font-weight: bold;">Select Image:</label>
          <select
            id="imageSelector"
            v-model="block.selectedImage"
            @change="event => onImageChange(event, block.id)"
            class="image-selector"
            style="width: 100%; padding: 8px; margin-bottom: 10px;"
          >
            <option v-for="image in predefinedImages" :key="image.value" :value="image.value">
              {{ image.label }}
            </option>
          </select>
          <div class="image-container" style="text-align: center;">
            <img :src="block.selectedImage" alt="Selected Image" style="max-width: 100%; border-radius: 8px;" />
          </div>
        </div>
        <!-- Duplicate and Delete Buttons -->
        <div style="margin-top: 10px;">
          <button @click="duplicateBlock(block)" style="width: 100%; margin-bottom: 5px;">Duplicate</button>
          <button @click="deleteBlock(block.id)" style="width: 100%;">Delete</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* General Styles */
#demo {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f0f0f0;
  gap: 20px; /* Add spacing between components */
}

/* Draggable Box Styles */
.draggable-box {
  border: 2px solid #6b7280;
  border-radius: 8px;
  background-color: #ffffff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 350px; /* Increased width */
  min-height: 300px; /* Increased height */
  overflow: hidden; /* Prevent content from overflowing */
}

.cursor {
  cursor: grab;
  font-weight: bold;
}

/* Editable Text Box */
.editable-textbox {
  font-size: 16px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
  background-color: #f9f9f9;
  color: #333;
  width: 100%;
  resize: none;
}
.editable-textbox:focus {
  border-color: #4a90e2;
  background-color: #fff;
}

/* Image Selector */
.image-selector {
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
  background-color: #f9f9f9;
  color: #333;
}

.image-selector:focus {
  border-color: #4a90e2;
  background-color: #fff;
}

/* Image Container */
.image-container img {
  max-width: 100%; /* Ensure image fits within the width */
  max-height: 200px; /* Limit height */
  border-radius: 4px;
  display: block; /* Prevent inline spacing issues */
  margin: 0 auto; /* Center the image */
}

/* Buttons */
button {
  margin-top: 5px;
  padding: 8px 16px;
  border: none;
  background-color: #4CAF50;
  color: white;
  font-size: 14px;
  cursor: pointer;
  text-align: center;
  display: block;
  border-radius: 4px; /* Rounded corners */
}
button:hover {
  background-color: #45a049;
}

</style>
