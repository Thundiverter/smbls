<script setup>
import { onMounted, ref } from 'vue';
import { useSearchStore } from '../stores/search';
const search = useSearchStore();
const searchInput = ref(null);

onMounted(() => {
    searchInput.value.focus();
})
</script>

<template>
    <input ref="searchInput" v-model="search.query" type="search" placeholder="Find a symbol...">
</template>

<style scoped>
input {
    --m: 4rem;
    margin: 0 var(--m);
    width: calc(100% - 2 * var(--m));
}
@media (max-width: 500px) {
    input {
        margin: 0;
        width: 100%;
    }
}
</style>