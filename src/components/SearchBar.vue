<template>
  <div class="relative max-w-xs">
    <input
      type="text"
      v-model="searchQuery"
      @keyup.enter="performSearch"
      placeholder="Search tenders..."
      class="pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 w-full"
    />

    <div
      class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
    >
      <svg
        class="w-5 h-5 text-gray-400"
        fill="none"
        stroke="currentColor"
        viewBox="0 0 24 24"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
        ></path>
      </svg>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "SearchBar",
  emits: ["search"],
  setup(props, { emit }) {
    const searchQuery = ref("");

    const performSearch = () => {
      emit("search", searchQuery.value);
    };

    return {
      searchQuery,
      performSearch,
    };
  },
});
</script>
