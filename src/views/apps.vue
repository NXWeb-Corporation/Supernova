<script setup>
import { reactive, onMounted } from 'vue'
import { search } from '@/assets/uv-stuff';

const data = reactive({
  items: [],
  iframe: null,
});

const fetchStuff = async () => {
  const response = await fetch('/apps.json')
  const apps = await response.json()
  data.items = apps
}

function go(url, type) {
  if (type == "uv") {
    let fixedurl = search(url);
    data.iframe = __uv$config.prefix + __uv$config.encodeUrl(fixedurl);
  } else {
    data.iframe = url;
  }
}

onMounted(fetchStuff)
</script>

<template>
  <div v-if="!data.iframe">
  <h1 class="font-rubik text-center text-title-blue text-7xl p-16">Apps</h1>
  <div class="flex flex-wrap justify-center">
    <div v-for="app in data.items" class="p-2">
      <button @click="go(app.url, app.type)"><img class="w-60 h-60 rounded-3xl" :src="app.img" :alt="app.alt"></button>
    </div>
  </div>
</div>
  <iframe v-if="data.iframe" :src="data.iframe" class="w-full h-screen absolute"></iframe>
</template>