<script setup>
import { ref } from "vue";
const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);
let ids = 1;

const getRandomColor = () => {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
};
const deleteCard = (index) => {
  notes.value.splice(index, 1);
};
const addNote = () => {
  if (newNote.value.length < 10) {
    return (errorMessage.value = "Note needs to be 10 characters or more");
  }
  notes.value.push({
    id: ids,
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });
  ids++;
  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
};
</script>
<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea
          v-model.trim="newNote"
          name="note"
          id=""
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote()">Add note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">{{ note.text }}</p>
          <div class="card-bottom">
            <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
            <button @click="deleteCard(note)">X</button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}
.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.75);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal {
  width: 750px;
  background-color: aliceblue;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: aliceblue;
  cursor: pointer;
  margin-top: 15px;
}
.modal .close {
  background-color: rgb(66, 59, 59);
  transition: 0.5s;
}
.modal p {
  color: red;
}
.close:hover {
  background-color: rgb(104, 95, 95);
  transition: 0.5s;
}
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}
header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: blueviolet;
  color: aliceblue;
  border-radius: 30px;
  transition: 0.5s;
  font-size: 20px;
  margin-right: 20px;
}
button:hover {
  background-color: rgb(174, 104, 240);
  transition: 0.5s;
}
.cards-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
}
.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin: 10px;
}

.date {
  font-size: 14px;
  font-weight: bold;
}
.card-bottom {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.card-bottom button {
  border-radius: 50%;
  border: none;
  background-color: inherit;
  cursor: pointer;
  transition: 0.4s;
}
.card-bottom button:hover {
  background-color: aliceblue;
  transition: 0.4s;
}
</style>
