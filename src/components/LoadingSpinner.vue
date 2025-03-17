<template>
  <div class="spinner"></div>
</template>

<script>
export default {
  name: 'LoadingSpinner'
};
</script>

<style scoped>
.spinner {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: 5px solid #ccc; /* Gray border */
  border-top: 5px solid #007BFF; /* Blue border-top */
  animation: spin 1s linear infinite;
  margin: 35% auto; /* Centering the spinner */
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
