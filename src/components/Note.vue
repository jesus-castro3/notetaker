<template>
  <div class="p-1">
    <h1 class="overflow-hidden font-bold truncate w-11/12">{{ name }}</h1>
    <p class="text-justify">{{ content }}</p>
  </div>
</template>

<script>
  export default {
    name: 'Note',
    props: {
      name: String,
      content: String,
    },
  }
</script>