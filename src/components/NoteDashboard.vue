<template>
  <section class="w-full overflow-auto">
    <ul class="grid xl:grid-cols-4 md:grid-cols-2 sm:grid-cols-1">
      <li
        @click="newNote" 
        class="relative flex justify-center items-center bg-gray-200 p-2 m-3 w-40 h-40 shadow-lg cursor-pointer border-gray-400 border">
        <span role="logo" class="text-6xl font-bold text-gray-600">+</span>
      </li>
      <li
        class="relative  p-2 m-3 w-40 h-40 overflow-hidden bg-yellow-200 text-xs shadow-lg cursor-pointer"
        v-for="note in noteList"
        :key="note.id"
        @click="onSelect(note.id)"
      >
        <button class="absolute right-0 top-0 p-2 font-bold" @click.stop="onDelete(note.id)">
          X
        </button>
        <Note :name="note.name" :content="note.content" />
      </li>
    </ul>
  </section>
</template>

<script>
  import Note from "./Note";
  export default {
    name: "note-dashboard",
    props: {
      noteList: Array,
      newNote: Function,
      onDelete: Function,
      onSelect: Function
    },
    components: {
      Note,
    },
  };
</script>