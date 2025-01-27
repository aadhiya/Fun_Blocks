<script lang="ts">
import { defineComponent } from 'vue-demi';

export default defineComponent({
  data() {
    return {
      currentEvent: {},
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
  },
});
</script>

<template>
  <div id="demo">
    <!-- Main draggable element -->
    <div v-draggable="{ handle: 'strong' }">
      <div style="display: flex; flex-direction: column; max-height: 140px">
        <strong class="cursor"> Drag here </strong>
        <div style="overflow: scroll">
          <div class="bg-gray-500" style="white-space: pre-wrap">
            I have long scrollable content and a handle ✋.
          </div>
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

.bg-gray-500 {
  background-color: #6b7280;
  color: white;
  padding: 1rem;
  border-radius: 8px;
}
</style>
