<script lang="ts">
import { defineComponent } from 'vue-demi';
import image1 from './assets/image1.jpg';
import image2 from './assets/image2.jpg';
import image3 from './assets/image3.jpg';

export default defineComponent({
  data() {
    return {
      blocks: [
        { id: 1, type: 'text', editableText: 'I am editable text. Drag this box around and edit me!', width: 350, height: 250 },
        { id: 2, type: 'image', selectedImage: image1, width: 350, height: 380 },
      ],
      predefinedImages: [
        { label: 'Image 1', value: image1 },
        { label: 'Image 2', value: image2 },
        { label: 'Image 3', value: image3 },
      ],
      resizingBlock: null,
      resizeStartX: 0,
      resizeStartY: 0,
    };
  },
  methods: {
    onImageChange(event, blockId) {
      const block = this.blocks.find((b) => b.id === blockId);
      if (block) block.selectedImage = event.target.value;
    },
    duplicateBlock(block) {
      const newBlock = { ...block, id: this.blocks.length + 1 };
      this.blocks.push(newBlock);
    },
    deleteBlock(blockId) {
      this.blocks = this.blocks.filter((block) => block.id !== blockId);
    },
    saveData() {
      const data = this.blocks.map((block) => ({
        id: block.id,
        type: block.type,
        content: block.type === 'text' ? block.editableText : block.selectedImage,
        width: block.width,
        height: block.height,
      }));
      console.log('Landing Page Data:', JSON.stringify(data, null, 2));
    },
    startResize(event, block) {
      this.resizingBlock = block;
      this.resizeStartX = event.clientX;
      this.resizeStartY = event.clientY;
      document.addEventListener('mousemove', this.resizeBlock);
      document.addEventListener('mouseup', this.stopResize);
    },
    resizeBlock(event) {
      if (!this.resizingBlock) return;
      const dx = event.clientX - this.resizeStartX;
      const dy = event.clientY - this.resizeStartY;
      this.resizingBlock.width = Math.max(300, this.resizingBlock.width + dx);
      this.resizingBlock.height = Math.max(250, this.resizingBlock.height + dy);
      this.resizeStartX = event.clientX;
      this.resizeStartY = event.clientY;
    },
    stopResize() {
      this.resizingBlock = null;
      document.removeEventListener('mousemove', this.resizeBlock);
      document.removeEventListener('mouseup', this.stopResize);
    },
  },
});
</script>

<template>
  <div id="app" class="flex flex-col items-center min-h-screen bg-gray-100 p-6 space-y-6">
    <!-- Save Button -->
    <button
      @click="saveData"
      class="px-6 py-3 bg-blue-500 text-white font-bold rounded-lg shadow-md hover:bg-blue-600 transition"
    >
      Save
    </button>

    <!-- Blocks -->
    <div
      v-for="block in blocks"
      :key="block.id"
      v-draggable="{ handle: 'strong' }"
      class="relative bg-white shadow-md rounded-lg border border-gray-400 p-4 flex flex-col"
      :style="{ width: block.width + 'px', minHeight: block.height + 'px' }"
    >
      <strong class="cursor-move block text-lg font-semibold">Drag here</strong>

      <!-- Text Block -->
      <div v-if="block.type === 'text'" class="flex flex-col w-full h-full">
        <textarea v-model="block.editableText"
          class="editable-textbox w-full h-full p-2 border rounded-md resize-none"></textarea>
      </div>

      <!-- Image Block -->
      <div v-if="block.type === 'image'" class="flex flex-col w-full h-full">
        <label class="font-semibold mt-2">Select Image:</label>
        <select v-model="block.selectedImage" @change="event => onImageChange(event, block.id)"
          class="w-full p-2 border rounded-md">
          <option v-for="image in predefinedImages" :key="image.value" :value="image.value">
            {{ image.label }}
          </option>
        </select>
        <div class="image-container mt-2 flex-grow">
          <img :src="block.selectedImage" alt="Selected Image" class="rounded-md w-full h-auto object-cover" />
        </div>
      </div>

      <!-- Buttons (Now placed BELOW the image, inside the block) -->
      <div class="mt-2 flex justify-between">
        <button @click="duplicateBlock(block)" class="btn-green w-[48%]">Duplicate</button>
        <button @click="deleteBlock(block.id)" class="btn-red w-[48%]">Delete</button>
      </div>

      <!-- Resizer -->
      <div class="absolute bottom-1 right-1 w-5 h-5 bg-gray-500 cursor-se-resize" @mousedown="startResize($event, block)">
      </div>
    </div>
  </div>
</template>

<style>
/* Tailwind Utility Classes */
#app {
  @apply flex flex-col items-center min-h-screen bg-gray-100 gap-6 p-6;
}

/* Draggable Box */
.draggable-box {
  @apply border border-gray-400 bg-white rounded-lg shadow-lg p-4 relative flex flex-col;
}

/* Editable Text Box */
.editable-textbox {
  @apply text-gray-700 bg-gray-50 border border-gray-300 rounded-md;
}
.editable-textbox:focus {
  @apply border-blue-500 bg-white;
}

/* Buttons */
.btn-green {
  @apply px-4 py-2 bg-green-500 text-white rounded-md hover:bg-green-600 transition;
}
.btn-red {
  @apply px-4 py-2 bg-red-500 text-white rounded-md hover:bg-red-600 transition;
}

/* Resizable Handle */
.cursor-se-resize {
  cursor: se-resize;
  @apply bg-gray-400;
}

/* Image Container */
.image-container img {
  @apply w-full h-auto rounded-md border object-contain;
}
</style>
