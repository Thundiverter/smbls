<script setup>
import { onMounted, ref } from 'vue';

import { useSearchStore } from '../stores/search';
const search = useSearchStore();

const filePath = "https://raw.githubusercontent.com/Thundiverter/smbls/main/list.json";
const list = ref(null);

function getData() {
    fetch(filePath)
        .then(response => response.json())
        .then(data => (list.value = data));
}
getData();


async function copy(symbol) {
    try {
        await navigator.clipboard.writeText(symbol.s);
        console.log('Symbol copied to clipboard');
    } catch (err) {
        console.error('Failed to copy: ', err);
    }
}

function updateList(query) {
    text.value = query;
}

function hasTag(symbol) {
    let flag = true;
    for (let q of search.query.split(' ')) {
        if (q.trim().length == 0) { return true }

        if (!symbol.tags.join(' ').includes(q)) {
            flag = false;
            break;
        }
    }

    return flag
}
</script>

<template>
    <!-- <div class="category" v-show="history.length > 0">
        <h3>Latest copied<button class="clearHistory" @click="clearHistory">Clear</button></h3>
        <div class="list">
            <button v-for="i of history.slice(0, 10)" :title="i.tags.join(', ')" @click="copy(i)">{{ i.s }}</button>
        </div>
    </div> -->

    <div v-for="section of list">
        <div class="category" v-if="section.symbols.filter((s) => hasTag(s)).length != 0">
            <h3>{{ section.title }}</h3>
            <div class="list">
                <button v-for="i of section.symbols.filter((s) => hasTag(s))" :title="i.tags.join(', ')" @click="copy(i)">{{ i.s }}</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.list {
    --cols: 10;
    margin: .75rem 0;
    display: grid;
    grid-template-columns: repeat(var(--cols), 1fr);
    gap: .5rem;
}
.list button {
    font-size: 1.5rem;
    padding: .3rem .7rem;
}
.category {
    margin-bottom: 2rem;
}

h3 {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.clearHistory {
    border: none;
    padding: .2rem .4rem;
    font-size: .8rem;
    color: gray;
}

@media (max-width: 700px) {
    .list {
        --cols: 6;
    }
}
@media (max-width: 500px) {
    .list {
        --cols: 5;
    }
}
</style>