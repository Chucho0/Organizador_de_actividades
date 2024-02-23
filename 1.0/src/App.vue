<template>
  <div class="padre">
    <div class="caja">
      <h3 :style="alerta === 'Actividad guardada' ? ['background:black', 'color:white', 'padding:8px'] : ''">{{ alerta }}</h3>
      <h1>Agendador de actividades</h1>
      <input class="input" v-model="tareas" type="text" placeholder="Agenda tu actividad" />
      <input class="input" v-model="fecha" type="date" />
    </div>

    <div class="opciones">
      <button @click="ordenar()" class="ordenar">Ordenar</button>
      <button @click="enviar()">Guardar</button>
      <label class="container">
        <input :checked="prioridad === 'alto'" @change="prioridad = $event.target.checked ? 'alto' : 'baja'"
          v-model="prioridad" type="checkbox" />
        <div class="checkmark"></div>
      </label>
    </div>

    <table>
      <colgroup>
        <col style="width: 250px" />
        <col style="width: 250px" />
        <col style="width: 250px" />
        <col style="width:200px" />
      </colgroup>
      <thead>
        <tr>
          <td>Actividad</td>
          <td>Prioridad</td>
          <td>fecha</td>
          <td>Opciones</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in datos" :key="index"
          :style="item.prioridad > 'alto' ? '' : ['background:red', 'color:white']">
          <td>{{ item.tareas }}</td>
          <td>{{ item.prioridad }}</td>
          <td>{{ item.fecha }}</td>
          <td>
            <button @click="eliminar(index)">❌</button>
            <button @click="editar(item,index)">📝</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref } from "vue";
let tareas = ref("");
let fecha = ref("");
let prioridad = ref("baja");

let datos = ref([]);
let alerta = ref("");

function enviar() {
  if (tareas.value === "") {
    alerta.value = "Agenda tu actividad";
    ocultar();
  } else if (fecha.value === "") {
    alerta.value = " ingresa fecha";
    ocultar();
  } else {
    datos.value.push({
      tareas: tareas.value,
      fecha: fecha.value,
      prioridad: prioridad.value,
    });
    alerta.value = "Actividad agendada";
    ocultar()
    console.log(datos);
    tareas.value = "";
    fecha.value = "";
  }
}

function ocultar() {
  setTimeout(() => {
    alerta.value = "";
  }, 1500);
}

const ordenar = () => {
  datos.value.sort((a, b) => {
    return a.prioridad === 'alto' ? -1 : 1;
  });
}

const eliminar=(index)=>{
  datos.value.splice(index,1)
}

const editar = (item, index) => {

};

</script>

<style scoped>
.caja {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 200vh;
  height: auto;
  padding: 20px;
}

.padre {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.input {
  width: 80%;
  padding: 10px;
  margin: 8px;
}

button {
  width: 20%;
}

table {
  text-align: center;
  background-color: black;
  padding: 10px;
  font-family: "Trebuchet MS", "Lucida Sans Unicode", "Lucida Grande",
    "Lucida Sans", Arial, sans-serif;
  margin: 30px;
  width: 200vh;
}

tbody {
  background-color: gray;
  border: 2px solid black;
}

tbody tr td {
  border-radius: 30px solid;
}

.container input {
  display: none;
}

.container {
  display: flex;
  position: relative;
  cursor: pointer;
  font-size: 20px;
  user-select: none;
  -webkit-tap-highlight-color: transparent;
  margin: 2px;
}

.checkmark {
  position: relative;
  top: 0;
  left: 0;
  height: 1.3em;
  width: 1.3em;
  background-color:white;
  border-radius: 0.25em;
  transition: all 0.25s;
}

.container input:checked~.checkmark {
  background-color: red;
}

.checkmark:after {
  content: "";
  position: absolute;
  transform: rotate(0deg);
  left: 0;
  top: 0;
  width: 1.05em;
  height: 1.05em;
  border-radius: 0.25em;
  transition: all 0.25s, border-width 0.1s;
}

.container input:checked~.checkmark:after {
  left: 0.45em;
  top: 0.25em;
  width: 0.25em;
  height: 0.5em;
  border-color: #fff0 gray gray #fff0;
  border-width: 0 0.15em 0.15em 0;
  border-radius: 0em;
  transform: rotate(45deg);
}

.opciones {
  display: flex;
  justify-content: space-around;
  width: 40%;
  padding: 10px;
  background-color: gray;
  border: 2px solid;
  margin: 30px;
  border-radius: 5px;
}

h3 {
  width: 400px;
  background: black;
  text-align: center;
  color: white;
}

</style>