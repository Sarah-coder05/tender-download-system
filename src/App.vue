<template>
  <div id="app" class="flex flex-col min-h-screen bg-gray-100 font-sans">
    <header
      class="bg-blue-700 text-white p-4 shadow-md fixed top-0 w-full z-10"
    >
      <nav class="container mx-auto flex justify-between items-center">
        <h1 class="text-xl md:text-2xl font-bold">Tender Download System</h1>
        <div v-if="user.isLoggedIn" class="flex items-center">
          <!-- Show search bar only if user is logged in -->
          <SearchBar @search="handleSearch" />
          <button
            @click="logout"
            class="ml-4 px-4 py-2 bg-blue-600 hover:bg-blue-800 rounded-lg transition duration-300 ease-in-out"
          >
            Logout
          </button>
        </div>
      </nav>
    </header>

    <main
      class="flex-grow pt-20 pb-8"
      :class="{
        'container mx-auto px-4': user.isLoggedIn, // Apply container styling when logged in
        'w-full flex items-center justify-center': !user.isLoggedIn, // Make main full width and center content when not logged in
      }"
    >
      <div v-if="user.isLoggedIn">
        <!-- Content for logged-in users -->
        <TenderList
          :searchQuery="currentSearchQuery"
          @view-details="openTenderModal"
        />
        <TenderDetailModal
          v-if="showTenderModal && selectedTender"
          :tender="selectedTender"
          :onClose="closeTenderModal"
        />
      </div>
      <div v-else class="flex justify-center items-center h-full">
        <!-- Show AuthForm if not logged in, listen for login-success -->
        <AuthForm @login-success="handleLoginSuccess" />
      </div>
      <TenderDetailModal
        v-if="showTenderModal && selectedTender"
        :tender="selectedTender"
        :onClose="closeTenderModal"
      />
    </main>

    <footer class="bg-gray-800 text-white text-center p-4 mt-8">
      <p>&copy; 2025 Tender Download System. All rights reserved.</p>
    </footer>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import TenderList from "./components/TenderList.vue";
import SearchBar from "./components/SearchBar.vue";
import AuthForm from "./components/AuthForm.vue";
import TenderDetailModal from "./components/TenderDetailModal.vue";

interface Tender {
  id: number;
  title: string;
  description: string;
  deadline: string;
  downloadLink: string;
}

export default defineComponent({
  name: "App",
  components: {
    TenderList,
    SearchBar,
    AuthForm,
    TenderDetailModal,
  },
  setup() {
    const currentSearchQuery = ref("");
    const user = ref({ isLoggedIn: false, email: "" });

    const showTenderModal = ref(false);
    const selectedTender = ref<Tender | null>(null);

    const handleLoginSuccess = (email: string) => {
      user.value.isLoggedIn = true;
      user.value.email = email;
      console.log(`User ${email} logged in successfully! (Static Auth)`);
    };

    const logout = () => {
      user.value.isLoggedIn = false;
      user.value.email = "";
      console.log("User logged out. (Static Auth)");
    };

    const handleSearch = (query: string) => {
      currentSearchQuery.value = query;
    };

    const openTenderModal = (tender: Tender) => {
      selectedTender.value = tender;
      showTenderModal.value = true;
    };

    const closeTenderModal = () => {
      showTenderModal.value = false;
      selectedTender.value = null; // Clear selected tender when closing
    };

    return {
      currentSearchQuery,
      handleSearch,
      user,
      logout,
      handleLoginSuccess,
      showTenderModal,
      selectedTender,
      openTenderModal,
      closeTenderModal,
    };
  },
});
</script>
