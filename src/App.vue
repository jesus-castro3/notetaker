<template>
  <main class="w-full h-screen flex  justify-between">
    <NoteDashboard :noteList="noteList" :onDelete="onDelete" :onSelect="onSelect" />
    <NoteEditor :onSubmit="onSubmit" :content="state.content" :name="state.name" @notechange="onChange"/>
  </main>
</template>

<script>
  import { onBeforeMount, reactive, ref } from 'vue';
  import NoteDashboard from './components/NoteDashboard';
  import NoteEditor from './components/NoteEditor';

  export default {
    name: 'App',
    setup() {
      const noteList = ref([]);

      const state = reactive({
        content: '',
        name: ''
      });

      function onSubmit() {
        if(!state.name.length) return;
        noteList.value.push({
          // In a real world situation we would use a unique id gen. from BE
          // Using Date.now for demo purposes
          id: Date.now(),
          name: state.name,
          content: state.content
        });
        state.name = '';
        state.content = '';
        //we save what we currently have to localstorage
        localStorage.setItem('noteTaker', JSON.stringify(noteList.value));
      }

      function onDelete(id) {
        const noteTakerStringData = localStorage.getItem('noteTaker');
        const noteTakerList = noteTakerStringData ? JSON.parse(noteTakerStringData) : [];
        const newNoteList = noteTakerList.filter(note => note.id !== id);
        noteList.value = newNoteList;
        localStorage.setItem('noteTaker', JSON.stringify(noteList.value))
      }

      function onSelect(id) {
        const noteTakerStringData = localStorage.getItem('noteTaker');
        const noteTakerList = noteTakerStringData ? JSON.parse(noteTakerStringData) : [];
        const newCurrentNote = noteTakerList.find(note => note.id === id);
        state.name = newCurrentNote.name;
        state.content = newCurrentNote.content;
      }
      
      function onChange(val, type){
        if(type === 'name') {
          state.name = val;
        } else {
          state.content = val;
        }
      }

       onBeforeMount(() => {
        const noteTakerStringData = localStorage.getItem('noteTaker');
        const noteTakerList = noteTakerStringData ? JSON.parse(noteTakerStringData) : [];
        noteList.value = noteTakerList;
      });
      
      return {
        onSubmit,
        onDelete,
        onSelect,
        onChange,
        noteList,
        state
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
