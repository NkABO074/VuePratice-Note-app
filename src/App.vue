<script setup>
import { ref } from "vue";

// states
const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref();
const notes = ref([]);

/**
 * return speudo-random light color
 * @returns {String} a light tone color 
 */
function getRandomLightColor() {
  const HUE = Math.random() * 360;
  return `hsl(${HUE}, 100%, 75%)`;
}

/**
 *  Append each note to the notes states array
 * @returns {String} if value inferior to 10 chars
 */
const addNote = () => {
  if(newNote.value.length < 10) {
    return errorMessage.value = "Notes need to be at least 10 characters long"
  }
  notes.value.push({
    id: Math.floor(Math.random * 10000000),
    text: newNote.value,
    date: new Date(),
    bg_color: getRandomLightColor(),
  });
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
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>

      <div class="card-container">

        <div v-for="note in notes" class="card" :style="{backgroundColor: note.bg_color}" :key="note.id">
          <p class="main-text">{{ note.text }} {{ note.id }}</p>
          <p class="date">
            {{ note.date.toLocaleDateString("en-US") }}
          </p>
        </div>

      </div>

    </div>
  </main>
</template>

<style scoped>
main {
  height: 100%;
  width: 100%;
  font-family: Montserrat, sans-serif;
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
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
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
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: fixed;
  width: 100vw;
  height: 100vh;
  background-color: rgb(0 0 0 /50%);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  backdrop-filter: blur(10px);
}

.modal {
  widows: 750px;
  background-color: whitesmoke;
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
  color: white;
  border: none;
  cursor: pointer;
  margin-top: 15px;
  border-radius: 10px;
  font-family: Montserrat, sans-serif;
}

.modal .close {
  background-color: red;
  margin-top: 7px;
}

.modal p {
  color:red;
}
</style>
