<script setup>

</script>

<template>
  <main class="px-4 py-12 sm:px-16 sm:py-16">
    <div font-sans p="x-4" text=" gray-700 ">
      <RouterView />
    </div>
    <TheFooter class="footer" />
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-flow: column;
  justify-content: center;
  min-height: 100%;
}

.footer {
  margin: auto auto 0;
}
</style>
