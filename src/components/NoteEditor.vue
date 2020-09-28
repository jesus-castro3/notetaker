<template>
  <section
    class="w-full p-20 flex text-left justify-center bg-gray-200 shadow-inner text-lx"
  >
    <form
      class="flex flex-col h-full w-full shadow-md border-solid border border-yellow-400"
      @submit.prevent="saveOrCreate"
    >
      <label class="sr-only" for="name-input">Name: </label>
      <input
        name="name-input"
        class="p-5 border-b-1 font-bold bg-yellow-200 outline-none border-gray-600"
        placeholder="Title"
        type="text"
        @input="$emit('notechange', $event.target.value, 'name')"
        :value="name"
      />
      <label class="sr-only" for="content-input">Content: </label>
      <textarea
        name="content-input"
        class="h-full px-5 bg-yellow-200 outline-none"
        placeholder="Content"
        @input="$emit('notechange', $event.target.value, 'content')"
        :value="content"
      />
      <button type="submit" class="p-1 font-bold bg-green-400 text-gray-200 hover:bg-green-500">
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