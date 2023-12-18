<template>
    <div class="note card">
        <div class="card-body">
            <input v-model="note.title" class="form-control" placeholder="Заголовок" />
            <textarea v-model="note.content" class="form-control" placeholder="Текст заметки"></textarea>
            <div class="form-check">
                <input type="checkbox" class="form-check-input" id="isImportant" v-model="note.isImportant" />
                <label class="form-check-label" for="isImportant">Важная заметка</label>
            </div>
            <div class="form-group">
                <label for="status">Статус:</label>
                <select class="form-control" id="status" v-model="note.status">
                    <option value="pending">Ожидает</option>
                    <option value="completed">Завершена</option>
                    <option value="inProgress">В процессе</option>
                </select>
            </div>
            <button @click="deleteNote" class="btn btn-danger">Удалить</button>
        </div>
    </div>
</template>
  
<script setup lang="ts">
import { ref, defineProps, getCurrentInstance } from 'vue';

const props = defineProps(['note']);
const ctx = getCurrentInstance();

const deleteNote = () => {
    ctx.emit('deleteNote', props.note.id);
};
</script>
  
<style scoped>
.note {
    margin-bottom: 20px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

textarea {
    width: 100%;
    margin-top: 5px;
}

select {
    margin-left: 5px;
}

/* Add Bootstrap classes */
label {
    display: block;
    margin-top: 10px;
}

.btn-danger {
    margin-top: 10px;
}
</style>