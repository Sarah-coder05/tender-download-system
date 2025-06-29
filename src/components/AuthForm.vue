<template>
  <div class="flex flex-col items-center justify-center">
    <div
      class="bg-white p-8 rounded-lg shadow-xl w-[500px] max-w-3xl border border-gray-200"
    >
      <h2 class="text-2xl font-bold mb-6 text-center text-gray-800">Login</h2>

      <form @submit.prevent="handleSubmit">
        <div class="mb-4">
          <label
            for="email"
            class="block text-gray-700 text-sm font-semibold mb-2"
            >Email</label
          >
          <input
            type="email"
            id="email"
            v-model="email"
            required
            class="shadow-sm appearance-none border rounded-lg w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
          />
        </div>

        <div class="mb-6">
          <label
            for="password"
            class="block text-gray-700 text-sm font-semibold mb-2"
            >Password</label
          >
          <input
            type="password"
            id="password"
            v-model="password"
            required
            class="shadow-sm appearance-none border rounded-lg w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
          />
        </div>

        <div
          v-if="error"
          class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative mb-4"
          role="alert"
        >
          <strong class="font-bold">Error!</strong>
          <span class="block sm:inline ml-2">{{ error }}</span>
        </div>

        <button
          type="submit"
          class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 transition duration-300 ease-in-out"
        >
          Login
        </button>
      </form>

      <!-- Removed the register toggle button -->
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "AuthForm",
  emits: ["login-success"], // Declare the new emit event
  setup(props, { emit }) {
    const email = ref("");
    const password = ref("");
    const error = ref<string | null>(null);

    // Hardcoded static credentials
    const STATIC_EMAIL = "user@ufedo.com";
    const STATIC_PASSWORD = "Abcd123";

    const handleSubmit = () => {
      error.value = null; // Clear previous errors

      if (email.value === STATIC_EMAIL && password.value === STATIC_PASSWORD) {
        emit("login-success", email.value); // Emit success event with email
        console.log("Static login successful!");
      } else {
        error.value = "Invalid email or password.";
        console.warn("Static login failed: Invalid credentials.");
      }
    };

    return {
      email,
      password,
      error,
      handleSubmit,
    };
  },
});
</script>

<style scoped>
/* No scoped styles needed */
</style>
