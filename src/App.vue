<template>
  <main class="w-full h-screen flex  justify-between">
    <NoteDashboard :noteList="noteList" :deleteNote="deleteNote" :selectNote="noteSelect" />
    <NoteEditor :onSubmit="onSubmit" ncontent="defaultvallll" name="default name"/>
  </main>
</template>

<script>
  import { onBeforeMount, ref } from 'vue';
  import NoteDashboard from './components/NoteDashboard';
  import NoteEditor from './components/NoteEditor';

  export default {
    name: 'App',
    setup() {
      const noteList = ref([]);
      const currentNote = ref({});
      function onSubmit(name, content) {
        if(!name.length) return;
        noteList.value.push({
          // In a real world situation we would use a unique id gen. from BE
          // Using Date.now for demo purposes
          id: Date.now(),
          name,
          content
        });
        //we save what we currently have to localstorage
        localStorage.setItem('noteTaker', JSON.stringify(noteList.value));
      }

      function deleteNote(id) {
        const noteTakerStringData = localStorage.getItem('noteTaker');
        const noteTakerList = noteTakerStringData ? JSON.parse(noteTakerStringData) : [];
        const newNoteList = noteTakerList.filter(note => note.id !== id);
        noteList.value = newNoteList;
        localStorage.setItem('noteTaker', JSON.stringify(noteList.value))
      }

      function noteSelect(id) {
        const noteTakerStringData = localStorage.getItem('noteTaker');
        const noteTakerList = noteTakerStringData ? JSON.parse(noteTakerStringData) : [];
        const newCurrentNote = noteTakerList.find(note => note.id === id);
        currentNote.value = newCurrentNote;
      }
      
       onBeforeMount(() => {
        const noteTakerStringData = localStorage.getItem('noteTaker');
        const noteTakerList = noteTakerStringData ? JSON.parse(noteTakerStringData) : [];
        noteList.value = noteTakerList;
      });
      
      return {
        onSubmit,
        deleteNote,
        noteList,
        noteSelect,
        currentNote
      }
    },

    components: {
      NoteDashboard,
      NoteEditor
    },
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
