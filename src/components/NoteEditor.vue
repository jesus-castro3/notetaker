<template>
  <section
    class="w-full p-20 flex text-left justify-center bg-gray-200 shadow-inner text-lx"
  >
    <form
      class="flex flex-col h-full w-full shadow-md border-solid border border-yellow-400"
      @submit.prevent="onSubmit(name, content)"
    >
      <input
        class="bg-yellow-200 outline-none p-5 border-b-1 border-gray-600"
        placeholder="Title"
        type="text"
        v-model="name"
      />
      <textarea
        class="bg-yellow-200 outline-none h-full px-5"
        placeholder="Content" 
        v-model="content"
      />
      <button class="bg-green-400 text-gray-200 font-bold p-1 hover:bg-green-500" type="submit">
        Create
      </button>
    </form>
  </section>
</template>

<script>
import { ref } from 'vue';
export default {
  name: 'NoteEditor',
  props: {
    onSubmit: Function,
  },
  setup(props) {
    console.log(props);
    const content = ref('');
    const name = ref('');

    return {
      content,
      name,
    };
  },
  
};
</script>