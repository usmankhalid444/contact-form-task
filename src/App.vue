<script setup>
import { ref, reactive } from "vue";

// Form data
const formData = reactive({
  name: "",
  email: "",
  message: "",
});

// Form errors
const errors = reactive({
  name: "",
  email: "",
  message: "",
});

// Form state
const isSubmitted = ref(false);
const isSubmitting = ref(false);

// Validate email format
const validateEmail = (email) => {
  const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return re.test(email);
};

// Validate form
const validateForm = () => {
  let isValid = true;

  // Reset errors
  errors.name = "";
  errors.email = "";
  errors.message = "";

  // Validate name
  if (!formData.name.trim()) {
    errors.name = "Name is required";
    isValid = false;
  }

  // Validate email
  if (!formData.email.trim()) {
    errors.email = "Email is required";
    isValid = false;
  } else if (!validateEmail(formData.email)) {
    errors.email = "Please enter a valid email address";
    isValid = false;
  }

  // Validate message
  if (!formData.message.trim()) {
    errors.message = "Message is required";
    isValid = false;
  }

  return isValid;
};

// Handle form submission
const handleSubmit = () => {
  if (validateForm()) {
    isSubmitting.value = true;

    // Simulate API call
    setTimeout(() => {
      isSubmitting.value = false;
      isSubmitted.value = true;

      // Reset form
      formData.name = "";
      formData.email = "";
      formData.message = "";
    }, 1000);
  }
};

// Reset form submission state
const resetForm = () => {
  isSubmitted.value = false;
};
</script>

<template>
  <div class="min-h-screen bg-gray-100 py-12 px-4 sm:px-6 lg:px-8">
    <div class="max-w-md mx-auto bg-white rounded-lg shadow-md overflow-hidden">
      <div class="px-6 py-8">
        <h2 class="text-2xl font-bold text-center text-gray-800 mb-8">
          Contact Us
        </h2>

        <!-- Success Message -->
        <div
          v-if="isSubmitted"
          class="bg-green-100 border-l-4 border-green-500 text-green-700 p-4 mb-6"
        >
          <div class="flex">
            <div class="flex-shrink-0">
              <svg
                class="h-5 w-5 text-green-500"
                fill="currentColor"
                viewBox="0 0 20 20"
              >
                <path
                  fill-rule="evenodd"
                  d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
                  clip-rule="evenodd"
                />
              </svg>
            </div>
            <div class="ml-3">
              <p class="text-sm font-medium">
                Thank you for your message! We'll get back to you soon.
              </p>
              <button
                @click="resetForm"
                class="mt-2 text-sm font-medium text-green-600 hover:text-green-500"
              >
                Send another message
              </button>
            </div>
          </div>
        </div>

        <!-- Contact Form -->
        <form v-else @submit.prevent="handleSubmit" class="space-y-6">
          <!-- Name Field -->
          <div>
            <label for="name" class="block text-sm font-medium text-gray-700"
              >Name</label
            >
            <input
              id="name"
              v-model="formData.name"
              type="text"
              class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500"
            />
            <p v-if="errors.name" class="mt-1 text-sm text-red-600">
              {{ errors.name }}
            </p>
          </div>

          <!-- Email Field -->
          <div>
            <label for="email" class="block text-sm font-medium text-gray-700"
              >Email</label
            >
            <input
              id="email"
              v-model="formData.email"
              type="email"
              class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500"
            />
            <p v-if="errors.email" class="mt-1 text-sm text-red-600">
              {{ errors.email }}
            </p>
          </div>

          <!-- Message Field -->
          <div>
            <label for="message" class="block text-sm font-medium text-gray-700"
              >Message</label
            >
            <textarea
              id="message"
              v-model="formData.message"
              rows="4"
              class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500"
            ></textarea>
            <p v-if="errors.message" class="mt-1 text-sm text-red-600">
              {{ errors.message }}
            </p>
          </div>

          <!-- Submit Button -->
          <div>
            <button
              type="submit"
              :disabled="isSubmitting"
              class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 disabled:opacity-50"
            >
              <span v-if="isSubmitting">Sending...</span>
              <span v-else>Submit</span>
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
