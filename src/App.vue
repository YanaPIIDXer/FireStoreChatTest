<script setup lang="ts">
import { onUnmounted } from 'vue';
import { initializeApp } from 'firebase/app';
import firebaseConfig from '@/assets/firebase.json';
import { getFirestore, doc, onSnapshot, DocumentSnapshot } from 'firebase/firestore';

const firebaseApp = initializeApp(firebaseConfig);
const db = getFirestore(firebaseApp);

const unsub = onSnapshot(doc(db, 'chat', 'message'), (snapshot: DocumentSnapshot) => {
  console.log(snapshot);
});

onUnmounted(() => unsub);
</script>

<template lang="pug">
.root
  h1 チャット
</template>

<style lang="sass" scoped>
</style>
