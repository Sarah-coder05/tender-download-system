<template>
  <div
    class="fixed inset-0 bg-gray-600 bg-opacity-75 flex items-center justify-center p-4 z-50"
  >
    <div
      class="bg-white rounded-lg shadow-xl p-6 w-full max-w-2xl relative animate-fade-in-up"
    >
      <!-- Close Button -->
      <button
        @click="onClose"
        class="absolute top-3 right-3 text-gray-500 hover:text-gray-800 text-2xl font-bold"
      >
        &times;
      </button>

      <h2 class="text-2xl font-bold mb-4 text-gray-900">{{ tender.title }}</h2>

      <div class="mb-4">
        <p class="text-gray-700 font-semibold mb-1">Description:</p>
        <p class="text-gray-800">{{ tender.description }}</p>
      </div>

      <div class="mb-4">
        <p class="text-gray-700 font-semibold mb-1">Submission Deadline:</p>
        <p class="text-gray-800">{{ tender.deadline }}</p>
      </div>

      <div class="mb-6">
        <p class="text-gray-700 font-semibold mb-1">Download Link:</p>
        <a
          :href="tender.downloadLink"
          target="_blank"
          class="text-blue-600 hover:underline inline-flex items-center"
          download
        >
          <svg
            class="w-5 h-5 mr-1"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"
            ></path>
          </svg>
          Download Document
        </a>
      </div>

      <div class="text-right">
        <button
          @click="onClose"
          class="px-5 py-2 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300 transition duration-300 ease-in-out"
        >
          Close
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";

interface Tender {
  id: number;
  title: string;
  description: string;
  deadline: string;
  downloadLink: string;
}

export default defineComponent({
  name: "TenderDetailModal",
  props: {
    tender: {
      type: Object as PropType<Tender>,
      required: true,
    },
    onClose: {
      type: Function as PropType<() => void>,
      required: true,
    },
  },
});
</script>

<style scoped>
/* Basic fade-in animation for the modal */
@keyframes fadeInScaleUp {
  from {
    opacity: 0;
    transform: scale(0.9);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.animate-fade-in-up {
  animation: fadeInScaleUp 0.3s ease-out forwards;
}
</style>
