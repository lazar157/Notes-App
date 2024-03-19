<template>
  <main>
    <div class="overlay" v-if="showModal">
      <div class="modal">
        <textarea name="note" id="note" cols="10" rows="10" maxlength="260" v-model.trim="newNote" @keyup.enter="addNote"></textarea>
        <p v-if="errorMsg">{{ errorMsg }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes <i class="fa-solid fa-book"></i></h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div class="card" v-for="(note, index) in notes" :key="note.id" :style="{ backgroundColor: note.backgroundColor }">
          <div class="popup">
            <div class="minibox">
              <i class="fa-solid fa-pen" @click="editNote(note)"></i>
              <i class="fa-solid fa-trash" @click="removeNote(index)"></i>
              <input type="text" class="editbox" v-model="note.text" @blur="doneEdit(note)" v-show="note == activeEdit">
            </div>
           

          </div>
          
              <p class="main-text">{{ note.text }}</p>
              <p class="date">{{ note.date }}</p>
         
          

        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { onMounted, ref } from 'vue';

onMounted(() => {
  const key = 'notes';
  const storedNotes = localStorage.getItem(key);
  if (storedNotes) {
    try {
      notes.value = JSON.parse(storedNotes);
    } catch (error) {
      console.error('Error parsing notes from localStorage:', error);
    }
  }
});

const showModal = ref(false)
const newNote = ref("");
const errorMsg = ref("")
const notes = ref([])
const activeEdit = ref(null)


function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";

}


const addNote = () => {

  if (newNote.value.length < 5) {
    return errorMsg.value = "Note needs to be 5 characters or more"
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date().toLocaleDateString("en-US"),
    backgroundColor: getRandomColor()

  })

  showModal.value = false
  newNote.value = ""
  errorMsg.value = ""

  const key = 'notes'
  localStorage.setItem(key, JSON.stringify(notes.value))




}

const removeNote = (index) =>{
  notes.value.splice(index, 1)
  const key = 'notes'
  localStorage.setItem(key, JSON.stringify(notes.value))
}

const editNote = (note) =>{
  activeEdit.value = note

}

const doneEdit = () =>{
  if(!activeEdit.value){
    return
  }
  activeEdit.value = null
  
  const key = 'notes';
  localStorage.setItem(key, JSON.stringify(notes.value));
}

  

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Shadows+Into+Light&family=Titan+One&display=swap');
textarea {
  width: 100%;
  height: 150px;
  padding: 12px 20px;
  box-sizing: border-box;
  border: 2px solid #9c6b6b;
  border-radius: 4px;
  background-color: #f8f8f8;
  font-size: 16px;
  resize: none;
  font-family: 'Poppins';
}

.main-text{
  line-height: 1.2;
}


.editbox{
  border-radius: 10px;
  position: absolute;
  left: 10px;
  top: 10px;
  border: 1px solid green;
  font-family: 'Poppins';
  padding-left: 10px;
  padding-right: 10px;

}

body {
  font-family: "Poppins", sans-serif;
  font-weight: 500;
  font-style: normal;
  padding: 0;
  margin: 0;
  background-image:url(slika/pozadina.jpg);
  background-size: 1000px;
  backdrop-filter: blur(1.5px);
  height: 100vh;
  
}

main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
  padding-left: 50px;
  background:rgb(241, 237, 238);
  box-shadow: -1px 2px 13px 0px rgba(0,0,0,0.75);
  
}

header {
  display: flex;
 justify-content: space-between;
  align-items: center;
  width: 95%;
  font-family: "Titan One", sans-serif;
  font-weight: 400;
  font-style: normal;
}



h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
  color: rgb(51, 51, 119);
 
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background: rgb(51, 51, 119);
  border-radius: 100%;
  color: white;
  font-size: 20px;
  
}

.card {
  width: 265px;
  padding-top: 35px;
  padding-left: 20px;
  padding-bottom: 20px;
  padding-right: 20px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
  transition: 0.5s;
  position: relative;
  overflow-wrap: break-word;
 
  

}

.card:hover {
  scale: 1.05;

}

.popup {
  
  opacity: 0;
  width: 100%;
  top: 3px;
  left:0px;
  height: 100%;
  transition: 0.5s;
  position: absolute;





}

.popup:hover {
  scale: 1.01;
  opacity: 1;

}

.popup i {
  position: relative;
  left: 200px;
  top: 0px;
  font-size: 15px;
  margin: 5px;
  border: 1px solid;
  padding: 6px;
  border-radius: 50%;
  color: rgb(51, 51, 119);
  background: rgb(241, 237, 238); 
cursor: pointer;


}

#note {
  resize: none;
}


.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
 
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;

}

.modal {
  width: 750px;
  background: white;
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
  background-color: rgb(56, 202, 105);
  border: none;
  color: white;
  cursor: pointer;
  margin-block: 15px;
}

.modal .close {
  background-color: rgb(193, 15, 15);
  margin-top: 7px;
}

.modal p {
  color: rgb(193, 15, 15);
}




</style>