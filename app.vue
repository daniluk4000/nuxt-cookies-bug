<template>
  <div>
    <div @click="cookie = !cookie">Set boolean cookie (click here): {{ cookie }}</div>
    <div @click="numberCookie !== 1 ? numberCookie = 1 : numberCookie = 2">Set number cookie (click here): {{ numberCookie }}</div>
    <div @click="textCookie !== 'test' ? textCookie = 'test' : textCookie = 'test123'">Set text cookie (click here): {{ textCookie }}</div>
    <div @click="!objectCookie ? objectCookie = {test: 'test'} : objectCookie.test !== 'test' ? objectCookie.test = 'test' : objectCookie.test = 'test123'">Set object cookie (click here): {{ objectCookie }}</div>

    <div>Real boolean cookie value: {{realCookie}}</div>
    <div>Real number cookie value: {{realNumberCookie}}</div>
    <div>Real text cookie value: {{realTextCookie}}</div>
    <div>Real object cookie value: {{realObjectCookie}}</div>
  </div>
</template>

<script setup>
import { nextTick } from 'vue';
const cookie = useCookie('test', {
  sameSite: 'lax',
});
const numberCookie = useCookie('numberTest', {
  sameSite: 'lax',
});
const textCookie = useCookie('textTest', {
  sameSite: 'lax',
});
const objectCookie = useCookie('objectTest', {
  sameSite: 'lax',
});
const realCookie = ref(null);
const realNumberCookie = ref(null);
const realTextCookie = ref(null);
const realObjectCookie = ref(null);

function getCookie(name) {
  const value = `; ${document.cookie}`;
  const parts = value.split(`; ${name}=`);
  if (parts.length === 2) return parts.pop().split(';').shift();
}

watch([cookie, numberCookie, textCookie, () => JSON.stringify(objectCookie.value ?? {})], () => {
  realCookie.value = getCookie('test')
  realNumberCookie.value = getCookie('numberTest')
  realTextCookie.value = getCookie('textTest')
  realObjectCookie.value = getCookie('objectTest')
})
</script>
