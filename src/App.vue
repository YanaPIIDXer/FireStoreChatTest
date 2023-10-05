<script setup lang="ts">
import { ref, onUnmounted } from 'vue';
import { initializeApp } from 'firebase/app';
import firebaseConfig from '@/assets/firebase.json';
import { getFirestore, query, onSnapshot, DocumentSnapshot, addDoc, collection, QuerySnapshot } from 'firebase/firestore';

const firebaseApp = initializeApp(firebaseConfig);
const db = getFirestore(firebaseApp);

interface Message {
  id: number;
  message: string;
}
const messages = ref<Message[]>([]);
const sendMessage = ref('');

const unsub = onSnapshot(query(collection(db, 'chat')), (snapshot: QuerySnapshot) => {
  console.log(snapshot);
});

onUnmounted(() => unsub);

/**
 * 送信
 */
const onSend = async (e: Event) => {
  e.preventDefault();

  await addDoc(collection(db, 'chat'), {
    id: messages.value.length + 1,
    message: sendMessage.value,
  });

  sendMessage.value = '';
}
</script>

<template lang="pug">
.root
  h1 チャット
  hr
  table(border='1')
    thead
      tr
        th Message
    tbody
      tr(v-for="(msg, index) in messages", :key="index")
        td {{ message }}
  hr
  form(@submit="onSend")
    input(type='text' v-model="sendMessage")
</template>

<style lang="sass" scoped>
</style>
