<script setup>
import { ref, watch } from "vue";

const notes = ref([
  { title: "Note 1", content: "This is the content of the first note." },
  { title: "Note 2", content: "This is the content of the second note." },
  { title: "Note 3", content: "This is the content of the third note." },
  { title: "Note 4", content: "This is the content of the fourth note." },
  { title: "Note 5", content: "This is the content of the fifth note." },
]);

let selectedNote = ref(null);
let newNoteTitle = ref("");
let newNoteContent = ref("");
let createNoteBool = ref(false);
let searchQuery = ref("");

const selectNote = (index) => {
  selectedNote.value = index;
};

const search = () => {
  return notes.value.filter((note) => {
    return note.title.toLowerCase().includes(searchQuery.value.toLowerCase());
  });
};

const createNote = async () => {
  createNoteBool.value = true;
  await selectNote(notes.value.length);
  saveNewNote();
};

const deleteNote = (index) => {
  notes.value.splice(index, 1);
  selectedNote.value = null;
};

const saveNewNote = () => {
  if (newNoteTitle.value.trim() === "") {
    newNoteTitle.value = "New Note";
  }

  const newNote = {
    title: newNoteTitle.value.trim(),
    content: newNoteContent.value.trim(),
  };

  notes.value.push(newNote);

  newNoteTitle.value = "";
  newNoteContent.value = "";
  createNoteBool.value = false;
};

const controlNotes = () => {
  notes.value.forEach((note, index) => {
    if (note.title === "New Note" && note.content === "") {
      deleteNote(index);
    }
  });
};

watch(selectedNote, () => {
  controlNotes();
});
</script>

<template>
  <div id="app" class="flex p-4 gap-3 bg-gray-100 bg">
    <div
      class="sidebar h-full p-4 divide-x"
      style="min-width: 300px; overflow-y: auto"
    >
      <div class="flex justify-between items-center w-full">
        <h2
          class="text-2xl font-bold text-gray-800 justify-center items-center"
        >
          Notes
        </h2>
        <button
          class="justify-center items-center plusbutton"
          @click="createNote()"
        >
          <svg
            class="h-8 w-8 text-yellow-500"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <line x1="12" y1="5" x2="12" y2="19" />
            <line x1="5" y1="12" x2="19" y2="12" />
          </svg>
        </button>
      </div>
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Search notes..."
        class="w-full p-2 rounded mb-4"
      />
      <ul class="w-full">
        <li
          v-for="(note, index) in search()"
          :key="index"
          @click="selectNote(index)"
          class="cursor-pointer py-2 flex justify-between items-center"
        >
          {{
            notes && note.title.length > 20
              ? note.title.slice(0, 20) + "..."
              : note.title
          }}
          <button @click="deleteNote(index)">
            <img src="../assets/delete.png" />
          </button>
        </li>
        <p v-if="search().length === 0" class="text-gray-500">
          No notes found.
        </p>
      </ul>
    </div>
    <div class="content flex-1 p-4">
      <div v-if="selectedNote !== null && notes[selectedNote]">
        <div class="topbar flex items-center justify-between mb-4">
          <div>
            <input
              type="text"
              v-model="notes[selectedNote].title"
              class="p-2 rounded mb-4 note-header"
              style="font-weight: bold; font-size: 24px"
            />
          </div>
        </div>
        <textarea
          v-model="notes[selectedNote].content"
          class="w-full p-2 rounded mb-4 bg-white max-h-screen"
        ></textarea>
      </div>
      <div v-else>
        <p class="text-gray-500">
          Please select a note or add a new one.
        </p>
      </div>
    </div>
  </div>
</template>

<style>
body {
  font-family: "Helvetica Neue", Arial, sans-serif;
}

#app {
  border: 1px solid #f5f4f4;
  border-radius: 5px;
  overflow: hidden;
  width: 100vw;
  height: 100vh;
}

.bg {
  background-image: url("../assets/bg.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}

.sidebar {
  position: sticky;
  top: 0;
  padding: 20px;
  background-color: #e8e9e8;
  border-radius: 20px;
}

.sidebar div {
  position: sticky;
  top: -20px;
  height: 60px;
  z-index: 1;
  background-color: #e8e9e8;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.sidebar input {
  border: none;
  border-radius: 4px;
  padding: 10px;
  width: 100%;
  height: 36px;
  margin-bottom: 20px;
  position: sticky;
  top: 40px;
  z-index: 1;
}

.sidebar h2 {
  font-size: 24px;
  font-weight: bold;
  color: #333;
}

.sidebar ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.sidebar li {
  cursor: pointer;
  padding: 10px;
  border-bottom: 1px solid #ddd;
  color: #333;
}

.sidebar li:hover {
  background-color: #d2d9dd;
}

.sidebar button {
  display: block;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 10px;
  cursor: pointer;
  height: 36px;
  width: 36px;
}

.sidebar .plusbutton {
  justify-content: center;
  align-items: center;
  padding: 0;
}

.content {
  flex: 1;
  padding: 20px;
  border-radius: 20px;
}

.content .note-header {
  font-size: 24px;
  font-weight: bold;
  color: #333;
  width: 80vw;
  background-color: transparent;
}

.content .note-header:focus {
  outline: none;
}

.create-note-form input {
  border: none;
  background-color: transparent;
}

.create-note-form input:focus {
  outline: none;
}

.create-note-form textarea {
  border: none !important;
  background-color: transparent;
}

.create-note-form textarea:focus {
  outline: none;
}

.content h2 {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
  color: #333;
}

.content textarea {
  width: 100%;
  height: 200px;
  padding: 10px;
  border: none;
  margin-bottom: 20px;
  background-color: transparent;
  resize: none;
  height: 80vh;
}
.content textarea:focus {
  outline: none;
}

.content button {
  background-color: #ffc107;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 10px;
  cursor: pointer;
}

.content button:hover {
  background-color: #ffa000;
}

.content p {
  color: #888;
}
</style>
