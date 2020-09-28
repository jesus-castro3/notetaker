<template>
  <section class="w-full overflow-scroll">
    <ul class="grid grid-cols-4">
      <li
        class="bg-yellow-200 p-2 m-3 w-40 h-40 text-xs relative overflow-auto shadow-lg cursor-pointer hover:border-solid"
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
  name: "NoteDashboard",
  props: {
    noteList: Array,
    onDelete: Function,
    onSelect: Function
  },
  components: {
    Note,
  },
};
</script>