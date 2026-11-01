<script setup>
  import { onMounted, ref } from "vue";
  const healthCheck = ref("");
  const loading = ref(false);
  const errorMessage = ref("");
  
  async function sendhealthCheck() {
    loading.value = true;

    try {
      const response = await fetch("/api/v1/health", {
        method: "GET",
        headers: { "Content-Type": "application/json" }
      });

      console.log("Response: ", response);

      if (!response.ok) {
        throw new Error(`HTTP Error: ${response.status}`);
      }

      const data = await response.json();

      healthCheck.value = data;

    } catch (error) {
      console.log("Error: ", "health check failed");

      errorMessage.value = error.message || "Health check failed";
    } finally {
      loading.value = false;
    }
  }

  onMounted(() => {
    sendhealthCheck();
  });
</script>

<template>
  <div>
    <p v-if="loading">
      Checking API health...
    </p>

    <p v-else-if="errorMessage" role="alert">
      Error: {{ ErrorMessage }}
    </p>

    <p v-else-if="healthCheck">
      API Status: {{ healthCheck }}
    </p>
  </div>
</template>