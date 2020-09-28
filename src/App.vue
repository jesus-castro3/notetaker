<template>
  <main class="flex justify-between w-full h-screen">
    <!-- App is separated into TWO sections LEFT is the dashboard with all your created notes -->
    <note-dashboard 
      :newNote="newNote" 
      :noteList="state.noteList" 
      :onDelete="onDelete" 
      :onSelect="onSelect" />
    <!-- RIGHT is your NoteEditor were you can either create or save a selected note -->
    <note-editor 
      :onSave="onSave"
      :onCreate="onCreate" 
      :content="state.content" 
      :name="state.name" 
      @notechange="onChange" 
      :id="state.id"/>
  </main>
</template>

<script>
  import { onBeforeMount, reactive } from 'vue';
  import NoteDashboard from './components/NoteDashboard';
  import NoteEditor from './components/NoteEditor';

  /** Went with using the new Composition API using the reactive state and setup method
   * replaces the need to declare things into data(), method() etc functionality
   *  **/
  export default {
    name: 'App',
    setup() {
      const state = reactive({
        id: '',
        content: '',
        name: '',
        noteList: []
      });

      function onSave() {
        state.noteList = state.noteList.map((note) => {
          if(note.id == state.id) {
            note.name = state.name;
            note.content =  state.content
          }
          return note;
        });
        //we save what we currently have to localstorage
        localStorage.setItem('noteTaker', JSON.stringify(state.noteList));      
      }

      function onCreate() {
        if(!state.name.length) return;
        state.noteList.push({
          // In a real world situation we would use a unique id gen. from BE
          // Using Date.now for demo purposes
          id: Date.now(),
          name: state.name,
          content: state.content
        });
        newNote();
        //we save what we currently have to localstorage
        localStorage.setItem('noteTaker', JSON.stringify(state.noteList));
      }

      function onDelete(id) {
        // we can mutate since we are working with reactivity
        state.noteList = state.noteList.filter(note => note.id !== id);
        localStorage.setItem('noteTaker', JSON.stringify(state.noteList))
      }

      function onSelect(id) {
        const newCurrentNote = state.noteList.find(note => note.id === id);
        state.id = newCurrentNote.id;
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

      function newNote() {
        state.content = '';
        state.name = '';
        state.id = '';
      }

      onBeforeMount(() => {
        const noteTakerStringData = localStorage.getItem('noteTaker');
        const noteTakerList = noteTakerStringData ? JSON.parse(noteTakerStringData) : [];
        state.noteList = noteTakerList;
      });

      
      return {
        onSave,
        onCreate,
        onDelete,
        onSelect,
        onChange,
        newNote,
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
