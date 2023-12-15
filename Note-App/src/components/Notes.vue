<script setup>
import { ref } from "vue";

const notes = ref([
  { title: "Not 1", content: "Bu birinci notun içeriğidir." },
  { title: "Not 2", content: "Bu ikinci notun içeriğidir." },
  { title: "Not 3", content: "Bu üçüncü notun içeriğidir." },
  { title: "Not 4", content: "Bu dördüncü notun içeriğidir." },
  { title: "Not 5", content: "Bu beşinci notun içeriğidir." },
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
  notes.value.filter((note) => {
    return note.title.includes(searchQuery.value);
  });
};

const createNote = () => {
  createNoteBool.value = true;
  selectedNote.value = null;
  newNoteTitle.value = "";
  newNoteContent.value = "";
};

const updateNote = () => {
  const updatedNote = notes[selectedNote.value];
};

const deleteNote = () => {
  notes.value.splice(selectedNote.value, 1);
  selectedNote.value = null;
};

const saveNewNote = () => {
  if (newNoteTitle.value.trim() !== "" && newNoteContent.value.trim() !== "") {
    const newNote = {
      title: newNoteTitle.value,
      content: newNoteContent.value,
    };
    notes.value.push(newNote);
    selectedNote.value = null;
  }
};
</script>

<template>
  <div id="app" class="flex p-4 bg-gray-100">
    <div class="sidebar w-1/3 border-r border-gray-300 p-4">
      <div class="flex justify-between items-center">
        <h2
          class="text-2xl font-bold text-gray-800 justify-center items-center"
        >
          Notlar
        </h2>
      </div>
      <ul>
        <li
          v-for="(note, index) in notes"
          :key="index"
          @click="selectNote(index)"
          class="cursor-pointer py-2 border-b border-gray-200 hover:bg-gray-200"
        >
          {{ note.title }}
        </li>
      </ul>
    </div>
    <div class="content flex-1 p-4">
      <div v-if="selectedNote !== null">
        <div class="topbar flex items-center justify-between mb-4">
          <div>
            <h2 class="text-2xl font-bold text-gray-800">
              {{ notes[selectedNote].title }}
            </h2>
          </div>
          <div class="flex">
            <button @click="createNote" class="new">
              <img src="../../public/deneme.svg" />
            </button>
            <button @click="search" class="search">
              <img src="../../public/search.svg" />
            </button>
            <button @click="deleteNote" class="delete">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-6 w-6"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M6 18L18 6M6 6l12 12"
                />
              </svg>
            </button>
          </div>
        </div>
        <textarea
          v-model="notes[selectedNote].content"
          class="w-full h-40 p-2 rounded mb-4 bg-white"
        ></textarea>
      </div>
      <div v-else-if="createNoteBool">
        <div class="create-note-form">
          <input
            v-model="newNoteTitle"
            type="text"
            id="newNoteTitle"
            class="w-full p-2 mb-4 rounded"
          />
          <textarea
            v-model="newNoteContent"
            id="newNoteContent"
            class="w-full h-40 p-2 mb-4 first-letter:rounded"
          ></textarea>

          <button
            @click="saveNewNote"
            class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600"
          >
            Kaydet
          </button>
        </div>
      </div>
      <div v-else>
        <p class="text-gray-500">
          Lütfen bir not seçin veya yeni bir not ekleyin.
        </p>
      </div>
    </div>
  </div>
</template>

<style>
body {
  background-color: #f5f4f4;
  font-family: "Helvetica Neue", Arial, sans-serif;
}

#app {
  border: 1px solid #f5f4f4;
  border-radius: 5px;
  overflow: hidden;
}

.sidebar {
  background-color: #d7d6d9;
  width: 300px;
  padding: 20px;
  border-radius: 20px;
}

.sidebar div {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
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
  background-color: #94999d;
}

.sidebar button {
  display: block;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 10px;
  cursor: pointer;
}

.sidebar button:hover {
  background-color: #45a049;
}

/* content style */
.content {
  background-color: #fcf4ed;
  flex: 1;
  padding: 20px;
  border-radius: 20px;
}

.content .new {
  background-color: #9d4edd;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 10px;
  cursor: pointer;
  width: 45px;
  margin-right: 10px;
}

.content .new:hover {
  background-color: #7d3caf;
}

.content .search {
  background-color: #7fc8f8;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 10px;
  cursor: pointer;
  width: 45px;
  margin-right: 10px;
}

.content .search:hover {
  background-color: #5aa9e6;
}

.content .delete {
  background-color: #e5383b;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 10px;
  cursor: pointer;
  width: 45px;
}

.content .delete:hover {
  background-color: #ba181b;
}

.create-note-form input {
  background-color: #fcf4ed;
  border: lightgray 1px solid;
  border-radius: 10px;
}

.create-note-form textarea {
  background-color: #fcf4ed;
  border: lightgray 1px solid !important;
  border-radius: 10px !important;
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
  background-color: #fcf4ed;
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
