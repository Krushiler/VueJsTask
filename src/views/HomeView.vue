<template>
  <main>
    <button @click="addNote" class="btn btn-primary mb-3">Добавить заметку</button>

    <div v-if="notes.length">
      <Note v-for="note in notes" :key="note.id" :note="note" @deleteNote="deleteNote" />
    </div>

    <div v-else>
      <p>Нет заметок</p>
    </div>

    <div class="mt-3">
      <button @click="clearStorage" class="btn btn-danger mr-2">Сбросить сохраненные данные</button>
      <button @click="saveNotesToStorage" class="btn btn-primary mr-4">Сохранить</button>
    </div>
  </main>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import Note from '../components/Note.vue';

const notes = ref([]);

const loadNotesFromStorage = () => {
  const storedNotes = sessionStorage.getItem('notes');
  if (storedNotes) {
    notes.value = JSON.parse(storedNotes);
  }
};

loadNotesFromStorage();

const saveNotesToStorage = () => {
  sessionStorage.setItem('notes', JSON.stringify(notes.value));
};

const addNote = () => {
  const newNote = {
    id: Date.now(),
    title: '',
    content: '',
    isImportant: false,
    status: 'pending',
  };
  notes.value.push(newNote);
  saveNotesToStorage();
};

const deleteNote = (noteId) => {
  console.log("pizda");
  notes.value = notes.value.filter((note) => note.id !== noteId);
  saveNotesToStorage();
};

const clearStorage = () => {
  sessionStorage.removeItem('notes');
  notes.value = [];
};

onMounted(() => {
  loadNotesFromStorage();
});
</script>

<style scoped>
main {
  max-width: 600px;
  margin: 0 auto;
}

button {
  margin-top: 10px;
  padding: 8px;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}

.btn-primary {
  margin-right: 10px;
}

.btn-danger {
  margin-top: 10px;
}

.mb-3 {
  margin-bottom: 1rem;
}

.mt-3 {
  margin-top: 1rem;
}

.mr-4 {
  margin-left: 0.5rem;
}
</style>