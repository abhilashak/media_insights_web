<script setup>
  import { onMounted, ref } from "vue";
  const healthCheck = ref("");
  const loading = ref(false);
  
  async function sendhealthCheck() {
    loading.value = true;
    try {
      const response = await fetch("/api/v1/health", {
        method: "GET",
        headers: { "Content-Type": "application/json" }
      });
      const data = await response.json();
      healthCheck.value = data;
      console.log(healthCheck.status);
    } catch (error) {
      console.log("Error: ", "health check failed");
    } finally {
      loading.value = false;
    }
  }

  onMounted(async () => {
    sendhealthCheck();
  });
</script>

<template>
  <div>
    {{ loading ? "checking health...." : healthCheck }}
  </div>
</template>