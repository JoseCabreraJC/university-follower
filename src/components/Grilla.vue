<template>
  <p>
    <ul>
      Aprobadas
      <span v-for="mat in state.aprobadas" :key="mat">- {{mat}}</span>
    </ul>
    <ul>
      Regulares
      <span v-for="mat in state.regularizadas" :key="mat">- {{mat}}</span>
    </ul>
  </p>
  <table class="table">
    <thead>
      <tr>
        <th>Orden</th>
        <th>Materia</th>
        <th>Regularizada</th>
        <th>Aprobada</th>
        <th>Puede Cursar</th>
        <th>Puede Rendir</th>
      </tr>
    </thead>
    <tbody>
      <Materia
        :materia="materia"
        v-for="materia in state.materias"
        :key="materia.id"
        @actualizarMateria="actualizarMateria"
      ></Materia>
    </tbody>
  </table>
</template>

<script setup>
import { defineProps, reactive, watch, watchEffect, toRef } from "vue";
import Materia from "./Materia.vue";

defineProps({
  msg: String,
});
function actualizarMateria({value, id, tipo}) {
  if (tipo === 'aprobar') {
    //afecta a aquellas materias que se pueden cursar y rendir
    let arreglo = []
    if (value) {
      state.aprobadas.push(id)
    } else {
      arreglo = state.aprobadas.filter(materia => materia !== id)
      state.aprobadas = arreglo
    }
    //actualziar materias que se pueden rendir y aprobar
    state.materias.forEach((materia) => {
      let puedeCursar; 
      puedeCursar = materia.aprobadasParaCursar
        .every((materiaDeseada) => {
          return state.aprobadas.includes(materiaDeseada)
        })
      materia.puedeCursar = puedeCursar
    })
      //agregar la  verificacion de su puede rendir   
    state.materias.forEach((materia) => {
      let puedeRendir; 
      // let regularizada = materia.regularizada; 
      puedeRendir = materia.paraRendir
        .every((materiaDeseada) => {
          return state.aprobadas.includes(materiaDeseada)
        })
        materia.puedeRendir = (materia.regularizada && puedeRendir)
      //agregar la  verificacion de su puede rendir   
      
    })
  } else if (tipo === 'regularizar') {
    let arreglo = []
    //o tambien llamadas cursadas. solo afectan a aquellas materias que se pueden cursar
      if (value) {
        state.regularizadas.push(id)
      } else {
        arreglo = state.regularizadas.filter(materia => materia !== id)
        state.regularizadas = arreglo
      }
      //actualizar materias que se pueden cursar
      state.materias.forEach((materia) => {
        let puedeCursar; 
        puedeCursar = materia.regularizadasParaCursar
          .every((materiaDeseada) => {
            return state.regularizadas.includes(materiaDeseada)
          })
          if (!materia.paraRendir.length && materia.id == id ) {
            materia.puedeRendir = value
          }
          materia.puedeCursar = puedeCursar && materia.aprobadasParaCursar
     
      })
    // lo mismo pero regular
  }
}
const state = reactive({
  totalMaterias: 0,
  aprobadas: [],
  regularizadas: [],
  materias: [
    //1ro
        { id: 40, nombre: "Primer Año", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },

    { id: 1, nombre: "Análisis Matematico 1", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },
    { id: 2, nombre: "Algebra y Geometria Analitica", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },
    { id: 3, nombre: "Matematica Discreta", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },
    { id: 4, nombre: "Sistemas y Organizaciones", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },
    { id: 5, nombre: "Algoritmos y Estructuras de Datos", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },
    { id: 6, nombre: "Arquitectura de Computadoras", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },
    { id: 7, nombre: "Fisica 1", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },
    { id: 8, nombre: "Ingles 1", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },
    //2do
    { id: 41, nombre: "Segundo Año", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },
    //2do
    { id: 9, nombre: "Quimica", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },
    { id: 10, nombre: "Análisis Matemático II", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [1, 2], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [1, 2] },
    { id: 11, nombre: "Física II", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [1, 7], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [1, 7] },
    { id: 12, nombre: "Análisis de Sistemas (Int.)", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [4, 5], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [4, 5] },
    { id: 13, nombre: "Sintaxis y Semántica de los Lenguajes", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [3, 5], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [3, 5] },
    { id: 14, nombre: "Paradigmas de Programación", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [3, 5], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [3, 5] },
    { id: 15, nombre: "Sistemas Operativos", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [3, 5, 6], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [3, 5, 6] },
    { id: 16, nombre: "Sistemas de Representacion", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },
    //3ro
    { id: 42, nombre: "Tercer Año", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },

{ id: 17, nombre: "Probabilidades y Estadísticas", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [1 ,2], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [1 , 2] },
    { id: 18, nombre: "Diseño de Sistemas (Int.)", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [12 , 14], 'aprobadasParaCursar':[3 , 4 , 5], 'regularizadasParaCursar': [12 , 14] },
    { id: 19 , nombre: "Comunicaciones", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [6 , 10 , 11], 'aprobadasParaCursar':[1 , 2 , 7 ], 'regularizadasParaCursar': [6 , 10 , 11] },
    { id: 20 , nombre: "Matematica Superior", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [10], 'aprobadasParaCursar':[1 , 2 ], 'regularizadasParaCursar': [10] },
    { id: 21 , nombre: "Gestion de Datos", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [12 , 13 , 14], 'aprobadasParaCursar':[3 , 4 , 5], 'regularizadasParaCursar': [12 , 13 , 14] },
    { id: 22 , nombre: "Ingenieria y Sociedad", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },
    { id: 23 , nombre: "Economia", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [12], 'aprobadasParaCursar':[4 , 5], 'regularizadasParaCursar': [12] },
    { id: 24 , nombre: "Ingles 2", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[8], 'regularizadasParaCursar': [] },
    //4to
        { id: 43, nombre: "Cuarto Año", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },

    { id: 25 , nombre: "Redes de Informacion", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [15 , 19], 'aprobadasParaCursar':[3 , 5 , 6 , 10 , 11], 'regularizadasParaCursar': [15 , 19] },
    { id: 26 , nombre: "Administracion de Recursos (Int.)", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [15 , 18 , 23], 'aprobadasParaCursar':[6 , 8 , 12 , 14], 'regularizadasParaCursar': [15 , 18 , 23] },
    { id: 27 , nombre: "Investigacion Operativa", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [17 , 20], 'aprobadasParaCursar':[10], 'regularizadasParaCursar': [17 , 20] },
    { id: 28 , nombre: "Simulacion", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [17 , 20], 'aprobadasParaCursar':[10], 'regularizadasParaCursar': [17 , 20] },
    { id: 29 , nombre: "Ingenieria de Software", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [17 , 18 , 21], 'aprobadasParaCursar':[12 , 13 , 14], 'regularizadasParaCursar': [17 , 18 , 21] },
    { id: 30 , nombre: "Teoria de Control", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [9 , 20], 'aprobadasParaCursar':[10 , 11 ], 'regularizadasParaCursar': [ 9 , 20] },
    { id: 31 , nombre: "Legislacion", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [12 , 22], 'aprobadasParaCursar':[4 , 5 ], 'regularizadasParaCursar': [12 , 22] },
    //5to
        { id: 44, nombre: "Quinto Año", regularizada: false, aprobada: false, puedeCursar: true, puedeRendir: false, 
    'paraRendir': [], 'aprobadasParaCursar':[], 'regularizadasParaCursar': [] },

    { id: 32 , nombre: "Proyecto Final (Int.)", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [1 ,2 ,3 ,4 ,5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16 , 17 , 18 , 19 , 21 , 22 , 23 , 24, 25, 26, 27, 28, 29, 30, 31, 33, 34, 35, 36 ], 'aprobadasParaCursar':[15 , 16 , 17 , 18 , 19 , 21 , 22 , 23 , 24], 'regularizadasParaCursar': [25 , 26 , 29 , 31] },
    { id: 33 , nombre: "Inteligencia Artificial", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [27 , 28 ], 'aprobadasParaCursar':[17 , 18 , 20], 'regularizadasParaCursar': [27 , 28] },
    { id: 34 , nombre: "Adminsitracion Gerencial", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [26 , 27], 'aprobadasParaCursar':[15 , 17 , 18 , 20 , 23], 'regularizadasParaCursar': [26, 27] },
    { id: 35 , nombre: "Sistemas de Gestion", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [26 , 27 , 28], 'aprobadasParaCursar':[15 , 17 , 18 , 20 , 23], 'regularizadasParaCursar': [26 , 27 , 28] },
    { id: 36 , nombre: "Practica Supervisada(PS)", regularizada: false, aprobada: false, puedeCursar: false, puedeRendir: false, 
    'paraRendir': [25 , 26 , 29 , 31], 'aprobadasParaCursar':[15 , 16 , 17 , 18 , 19 , 21 , 22 , 23 , 24], 'regularizadasParaCursar': [25 , 26 , 29 , 31] }],
}
)

const aprobadas = toRef(state,'aprobadas')
// watchEffect(() => {
//   console.log(state.aprobadas.value)
//   console.log('wacher?');
// });

</script>

<style scoped>
a {
  color: #42b983;
}
span {
display: inline-block;
}
</style>
