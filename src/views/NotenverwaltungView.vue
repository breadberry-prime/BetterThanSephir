<template>
<div>
  <div class="titleWrapper">
    <h1>BetterThanSephir</h1>
  </div>
  <div> 
    <div id="mainGrid" v-for="(noten, fach) in Fächer">
      <div class="heading-wrapper">
        <h3>{{fach}} Noten</h3>
      </div>
      <div>
          <div class="grade" v-for="(note, index) in noten.noten" :key="index">
            <span>
              {{ parseFloat(note).toFixed(2) }}
              <button @click="deleteNote(fach, index)">❌</button>
            </span>
          </div>
          <div>
            <p>{{ fach }} Ø: {{ parseFloat(calculateAverageGrade(fach)).toFixed(2) }}</p>
          </div>
          <div class="inputWrapper">
            <input class="gradeInput" type="number" min="0" max="6" @keyup.enter="addNote($event, fach)" />
            <h5>Press enter to register grade</h5>
          </div>
      </div>
    </div>
  </div>
</div>
</template>

<script setup>
import { ref } from "@vue/reactivity";

const Fächer = ref({
  GES: { noten: [4, 5.5] },
  M151: { noten: [2, 5, 4] },
  M152: { noten: [6, 6] },
  M153: { noten: [6] },
  M306: { noten: [6, 6] },
  NWS: { noten: [6, 6] },
  SPK: { noten: [4.5] },
  SPO: { noten: [5.1] },
  WUR: { noten: [5.3] },
});

function deleteNote(fach, note) {
  Fächer.value[fach].noten.splice(note, 1);
}

function addNote(e, fach) {
  let newNote = Math.min(6, Math.max(1, parseInt(e.target.value)));
  Fächer.value[fach].noten.push(newNote);
}

function calculateAverageGrade(fach) {
  let sum = 0;
  for (let note of Fächer.value[fach].noten) {
    sum += note;
  }
  return sum / Fächer.value[fach].noten.length;
}

function calculateAverageGradeFromAllSubjects() {
  let sum = 0;
  let count = 0;
  for (let fach in Fächer.value) {
    for (let note of Fächer.value[fach].noten) {
      sum += note;
      count++;
    }
  }
  return sum / count;
}

function addSubject(e) {
  Fächer.value[e.target.value] = { noten: [] };
}

function roundHalf(num) {
    return Math.round(num*2)/2;
}
</script>
<style lang="scss">
h1, h2, h3, h4, h5 {
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
}

.home {
  display: grid;
  min-height: 100vh;
  min-width: 100vw;
  margin: 0;
  justify-content: center;
}
ul#Fächer {
  width: 50%;
}

.titleWrapper{
    background-color: pink;
    height: 100px;
    justify-content: center;
    align-items: center;
    display: flex;
}

.gradeInput {
  width: 200px;
  height: 40px;
}

.inputWrapper{
  display: flex;
  justify-content: center;
  align-items: center;

  h5{
    padding-left: 10px;
  }
}

.grade {
  margin-top: 5px;
  margin-bottom: 5px;
}

button {
  border-radius: 0%;
  background-color: aliceblue;
  border: 0ch;
}


</style>