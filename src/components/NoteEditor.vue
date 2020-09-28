<template>
  <section
    class="w-full p-20 flex text-left justify-center bg-gray-200 shadow-inner text-lx"
  >
    <form
      class="flex flex-col h-full w-full shadow-md border-solid border border-yellow-400"
      @submit.prevent="saveOrCreate"
    >
      <input
        class="bg-yellow-200 outline-none p-5 border-b-1 border-gray-600 font-bold"
        placeholder="Title"
        type="text"
        @input="$emit('notechange', $event.target.value, 'name')"
        :value="name"
      />
      <textarea
        class="bg-yellow-200 outline-none h-full px-5"
        placeholder="Content"
        @input="$emit('notechange', $event.target.value, 'content')"
        :value="content"
      />
      <button class="bg-green-400 text-gray-200 font-bold p-1 hover:bg-green-500" type="submit">
        {{ submitButtonMessage }}
      </button>
    </form>
  </section>
</template>

<script>
/* eslint-disable vue/no-mutating-props */
import { ref, computed } from 'vue';
export default {
  name: 'note-editor',
  props: {
    onSave: Function,
    onCreate: Function,
    id: String,
    name: String,
    content: String
  },
  setup(props) {
    const submitButton = ref('Create');
    const submitButtonMessage = computed(() => props.id ? 'Save' : submitButton.value);
    const saveOrCreate = computed(() => props.id ? props.onSave : props.onCreate)
    return {
      submitButtonMessage,
      saveOrCreate
    }
  },
};
</script>