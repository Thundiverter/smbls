<script setup>
import Search from './components/Search.vue'
import SymbolsList from './components/SymbolsList.vue'
</script>

<template>
  <main>
    <section>
      <Search  />
    </section>
    <section>
      <SymbolsList />
    </section>
  </main>
</template>

<style scoped>
main {
  max-width: 960px;
  margin: 3rem auto;

  display: flex;
  flex-direction: column;
  gap: 3rem;
}
section {
  margin: 0 1rem;
}
</style>