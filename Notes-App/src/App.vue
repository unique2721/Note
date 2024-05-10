<script setup>
import { ref } from 'vue';
let showModal = ref(false);
let newNote = ref("")
let notes = ref([]);
let errorMessage = ref('')
let totalNumberOfNots = ref(0)

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNoteHandler = () => {
  if(newNote.value.trim() == "") {
    errorMessage.value = 'please type some notes to add'
    setTimeout(() => {
      errorMessage.value = ''
    }, 3000);
    return
  }
  else if(newNote.value.trim().length <= 3) {
    errorMessage.value = 'a mininmum of 4 characters is needed to add notes'
    setTimeout(() => {
      errorMessage.value = ''
    }, 3000);
    return
  }
  else {
    notes.value.push({
      id: Math.floor(Math.random() * 1000000),
      text: newNote.value.trim(),
      date: new Date().toLocaleDateString("en-us"),
      backgroundColor: getRandomColor()
    })
    newNote.value = "";
    showModal.value = false;
    totalNumberOfNots.value++
  }
}

const closeNoteHandler = () => {
  showModal.value = false;
  newNote.value = ''
}

const removeNote = (index) => {
  notes.value.splice(index,1);
  totalNumberOfNots.value--
}
</script>

<template>
  <div class="overlay" v-if="showModal">
    <div class="overlay-container">
      <textarea name="note" id="note" cols="30" rows="10" placeholder="type notes here..." v-model="newNote"></textarea>
      <p class="error">{{errorMessage}}</p>
      <button class="add" @click="addNoteHandler">Add Note</button>
      <button class="close" @click="closeNoteHandler">Close</button>
    </div>
  </div>
  <div class="container">
    <header>
      <h1>Notes</h1>
      <p v-if="totalNumberOfNots ===0">oops, You have no Notes</p>
      <p v-else-if="totalNumberOfNots === 1">You have added {{totalNumberOfNots}} note</p>
      <p v-else >You have added {{totalNumberOfNots}} notes</p>
      <button @click="showModal = !showModal" title="add note">+</button>
    </header>
    <div>

      <div v-if="totalNumberOfNots === 0" class="add-note no-note">
        <h3>Add notes by clicking the plus button at the top right of the corner</h3>
        <h1>Notes App</h1>
        <p>This is a Notes app designed by applying the following vue js concepts</p>
    <ul>
      <li class="first">Conditional Rendering</li>
      <li>Text Interpolation</li>
      <li>Two way bindnig {v-model}</li>
      <li>reactive states {ref}</li>
      <li>Event Handling in vue</li>
      <li>Loops in Vue {for loop}</li>
      <li class="last">Dynamic Binding</li>
    </ul>
      </div>
    </div>

    <div class="notes-container">
      <div class="notes" v-for="(note,index) in notes" :key="index" :style="{backgroundColor: note.backgroundColor}">
        <p>{{note.text}}</p>
        <div class="remove-btn-container">
          <p>{{note.date}}</p>
          <button class="remove-btn" @click="removeNote(index)">remove</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.no-note {
  display: flex;
  justify-content: center;
  align-items: center; 
  flex-direction: column;
}
.no-note h1 {
  font-size: 3rem;
  color: fuchsia;
}
.no-note p {
  font-size: 1.2rem;
  font-style: italic;
  margin-bottom: 20px;
}
.no-note ul {
  border-radius: 15px;
  width: 400px;
  text-align: center;
  margin-bottom: 40px;
}
.no-note ul li {
  list-style-type: none;
  font-size: 1.75rem;
  padding: 5px;
}
.no-note ul li:nth-of-type(even) {
  background-color: orange;
}
.no-note ul li:nth-of-type(odd) {
  background-color: rgba(0, 0, 255, 0.74);
  color: white;
}
.no-note ul li:hover {
  background-color: #6114E0;
  cursor: pointer;
  color: white;
}
.no-note ul .first {
  border-radius: 15px 15px 0 0 ;
}
.no-note ul .last {
  border-radius: 0 0 15px 15px;
}

.container {
  width: 100vw;
  height: 100vh;
}
.container header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: linear-gradient(aqua,blue);
  width: 100%;
  color: white;
  z-index: 100;
  position: sticky;
  top: 0;
}

.container header h1 {
  font-size: 4rem;
  font-weight: bolder;
  margin: 0px 200px;
}

.container header button {
  font-size: 2.5rem;
  border: none;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  cursor: pointer;
  text-align: center;
  margin: 0px 200px;
}

.container .notes-container {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}
.container .add-note {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.container .add-note h3 {
  animation: slide linear 10s infinite;
}
.container .notes-container .notes {
  width: 300px;
  height: 300px;
  border-radius: 15px;
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  background: red;
  flex-wrap: wrap;
  margin: 10px;
  overflow: hidden;
}
.container .notes-container .notes p {
  font-size: 1.5rem;
  padding-left: 10px;
}
.container .notes-container .notes .remove-btn-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.container .notes-container .notes .remove-btn-container .remove-btn {
  width: 100px;
  font-size: 1.2rem;
  overflow: hidden;
  box-shadow: 3px 3px 4px rgba(255, 255, 255, 0.801);
  cursor: pointer;
  font-style: italic;
  border: none;
  background: inherit;
  color: crimson;
}
.container .notes-container .notes .remove-btn-container .remove-btn:hover {
  box-shadow: none;
}

.overlay {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
  z-index: 100;
  background: rgba(0,0,0,0.75);
  position: absolute;
}

.overlay .overlay-container {
  width: 700px;
  height: 300px;
 background: rgba(255, 255, 255, 0.801);
  border-radius: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.overlay .overlay-container textarea {
  resize: none;
  width: 650px;
  border: 2px solid aqua;
  border-radius: 5px;
  margin-bottom: 5px;
  margin: 10px auto;
}
.overlay .overlay-container .add,.overlay .overlay-container .close {
  width: 650px;
  font-size: 1.2rem;
  font-weight: bold;
  padding: 8px;
  color: white;
  background-color: rgb(93, 9, 172);
  margin-bottom: 7px;
  cursor: pointer;
}
.overlay .overlay-container .close {
  background-color: rgba(240, 21, 21, 0.918);
  margin-bottom: 20px;
}
.overlay .overlay-container .add:hover {
  background-color: rgba(93, 9, 172,0.95);
}
.overlay .overlay-container .close:hover {
  background-color: rgba(240, 21, 21, 0.76);
}
.overlay .error {
  color: red;
  font-size: 1.2rem;
}

@keyframes slide {

0% {
transform: translateX(-30px);
}
30%{
transform: translateX(20px);
}
70%{
transform: translateX(10%);
}
90%{
transform: translateX(-10%)
}

100% {
transform: translateX(0px);}
  
}
</style>
