<template>
  <div class="container mx-auto px-4 py-8">
    <div v-if="loading" class="text-center text-blue-600 py-8">
      <p>Loading tenders...</p>
      <svg
        class="animate-spin h-8 w-8 text-blue-500 mx-auto mt-4"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
      >
        <circle
          class="opacity-25"
          cx="12"
          cy="12"
          r="10"
          stroke="currentColor"
          stroke-width="4"
        ></circle>
        <path
          class="opacity-75"
          fill="currentColor"
          d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
        ></path>
      </svg>
    </div>

    <div
      v-else-if="error"
      class="text-center text-red-600 py-8 border border-red-300 bg-red-50 rounded-lg p-6"
    >
      <p class="text-xl font-semibold mb-3">Oops! Something went wrong.</p>
      <p class="mb-4">Error: {{ error.message }}</p>
      <button
        @click="fetchData"
        class="bg-red-500 hover:bg-red-600 text-white font-semibold py-2 px-4 rounded-lg transition duration-300 ease-in-out"
      >
        Retry
      </button>
    </div>

    <div
      v-else-if="paginatedTenders.length === 0"
      class="text-center text-gray-600 py-8"
    >
      <p>No tenders found matching your search criteria on this page.</p>
      <p v-if="totalPages > 1 && searchQuery === ''">
        Try navigating to another page.
      </p>
    </div>

    <div
      v-else
      class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 mt-[100px] gap-6"
    >
      <div
        v-for="tender in paginatedTenders"
        :key="tender.id"
        class="bg-white rounded-lg shadow-md p-6 border border-gray-200 cursor-pointer hover:shadow-lg transition duration-200 ease-in-out"
        @click="viewDetails(tender)"
      >
        <h2 class="text-xl font-semibold mb-2 text-gray-900">
          {{ tender.title }}
        </h2>
        <p class="text-gray-700 mb-4">{{ tender.description }}</p>
        <div class="flex items-center text-gray-600 text-sm mb-4">
          <svg
            class="w-4 h-4 mr-1"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
            ></path>
          </svg>
          Submission Deadline: {{ tender.deadline }}
        </div>
        <a
          :href="tender.downloadLink"
          target="_blank"
          class="inline-block bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition duration-300 ease-in-out"
          @click.stop="() => {}"
          download
        >
          Download Tender
        </a>
      </div>
    </div>

    <div
      v-if="totalPages > 0 && !loading && !error"
      class="flex justify-center items-center space-x-4 mt-8"
    >
      <button
        @click="goToPage(currentPage - 1)"
        :disabled="currentPage === 1"
        class="px-4 py-2 bg-gray-300 text-gray-800 rounded-lg disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-400"
      >
        Previous
      </button>

      <span class="px-4 py-2 font-semibold text-lg text-gray-800">
        Page {{ currentPage }} of {{ totalPages }}
      </span>

      <button
        @click="goToPage(currentPage + 1)"
        :disabled="currentPage === totalPages"
        class="px-4 py-2 bg-gray-300 text-gray-800 rounded-lg disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-400"
      >
        Next
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed, toRefs, onMounted, watch } from "vue"; // Import 'watch'

interface Tender {
  id: number;
  title: string;
  description: string;
  deadline: string;
  downloadLink: string;
}

export default defineComponent({
  name: "TenderList",
  props: {
    searchQuery: {
      type: String,
      default: "",
    },
  },

  emits: ["view-details"],

  setup(props, { emit }) {
    const tenders = ref<Tender[]>([]);
    const loading = ref(true);
    const error = ref<Error | null>(null);

    const currentPage = ref(1);
    const itemsPerPage = ref(3);

    const fetchData = async () => {
      loading.value = true;
      error.value = null;

      try {
        const response = await fetch("/tenders.json");
        if (!response.ok) {
          throw new Error(
            `Failed to load tenders: ${response.status} ${response.statusText}`
          );
        }
        tenders.value = await response.json();
        currentPage.value = 1; // Reset to first page on new data/successful fetch
      } catch (err: any) {
        error.value = err;
        console.error("fetchData: Fetch error:", err);
      } finally {
        setTimeout(() => {
          loading.value = false;
        }, 3000);
      }
    };

    onMounted(fetchData);

    const { searchQuery } = toRefs(props);
    watch(searchQuery, () => {
      currentPage.value = 1;
    });

    // Filter tenders based on searchQuery
    const filteredTenders = computed(() => {
      if (!searchQuery.value) {
        return tenders.value;
      }
      const query = searchQuery.value.toLowerCase();
      return tenders.value.filter(
        (tender) =>
          tender.title.toLowerCase().includes(query) ||
          tender.description.toLowerCase().includes(query)
      );
    });

    // Calculate total pages
    const totalPages = computed(() => {
      return Math.ceil(filteredTenders.value.length / itemsPerPage.value);
    });

    // Get tenders for the current page
    const paginatedTenders = computed(() => {
      const startIndex = (currentPage.value - 1) * itemsPerPage.value;
      const endIndex = startIndex + itemsPerPage.value;
      return filteredTenders.value.slice(startIndex, endIndex);
    });

    // Method to change page
    const goToPage = (page: number) => {
      if (page >= 1 && page <= totalPages.value) {
        currentPage.value = page;
      }
    };

    const viewDetails = (tender: Tender) => {
      emit("view-details", tender);
    };

    return {
      tenders,
      filteredTenders,
      paginatedTenders,
      loading,
      error,
      currentPage,
      itemsPerPage,
      totalPages,
      goToPage,
      fetchData,
      viewDetails,
    };
  },
});
</script>
