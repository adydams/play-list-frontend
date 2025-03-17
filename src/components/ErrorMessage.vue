<template>
    <div class="text-red-600 mt-2">
      {{ message }}
    </div>
  </template>
  
  <script>
  export default {
    props: {
      message: String
    }
  };
  </script>
  
  <style scoped>
  /* Custom Styles for ErrorMessage */
  </style>
  