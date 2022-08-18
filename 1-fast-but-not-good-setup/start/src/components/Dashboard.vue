<script setup>
import { onMounted, reactive, onBeforeMount } from 'vue';
import { useRouter } from 'vue-router';
import { initializeApp } from 'firebase/app';
import { config } from '../config'
import { getFirestore, collection, doc, setDoc, onSnapshot } from 'firebase/firestore'; 

const app = initializeApp(config.firebase);

const db = getFirestore(app);
const markdownCol = collection(db, 'markdowns');
const state = reactive({ markdowns: [] });
const router = useRouter();

onBeforeMount(async () => {
  // Get a user
})

onMounted(() => {
  onSnapshot(markdownCol, snapshot => {
    state.markdowns = snapshot.docs.map((d) => ({id: d.id, ...d.data }))
  })
})

function newMarkdown() {
  const newId = Math.random().toString(36).substr(2, 5);
  router.push(`/editor/${newId}`)
}
</script>

<template>
  <h1>I am the dashboard</h1>

  <ul v-for="markdown in state.markdowns" :key="markdown.id">
    <li>
      <router-link :to="{ path: `/editor/${markdown.id}` }">{{ markdown.id }}</router-link>
    </li>
  </ul>

  <button @click="newMarkdown">New</button>

</template>
