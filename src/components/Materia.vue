<template>
  <tr
    v-if="materia.id < 40"
    :class="{
      regu: materia.regularizada && !materia.aprobada,
      apro: materia.aprobada,
      inhabilitado: !materia.puedeCursar && !materia.puedeRendir,
    }"
  >
    <td>{{ materia.id }}</td>
    <td>{{ materia.nombre }}</td>
    <td>
      <input
        @click="
          $emit('actualizarMateria', {
            id: materia.id,
            value: !materia.regularizada,
            tipo: 'regularizar',
          })
        "
        v-model="materia.regularizada"
        type="checkbox"
        name="regularChBx"
        id=""
      />
    </td>
    <td>
      <input
        @click="
          $emit('actualizarMateria', {
            id: materia.id,
            value: !materia.aprobada,
            tipo: 'aprobar',
          })
        "
        v-model="materia.aprobada"
        type="checkbox"
        name="aprobadaChBx"
        id=""
      />
    </td>
    <td>{{ materia.puedeCursar ? "Cursable" : "No cursable aún" }}</td>
    <td>{{ materia.puedeRendir ? "Rendible" : "No rendible aún" }}</td>
  </tr>
  <tr v-else>
    <td>{{ materia.nombre }}</td>
  </tr>
</template>

<script setup>
import { defineProps, defineEmit } from "vue";

// const emit = defineEmit({ aprobarMateria: {} });
// console.log(context);

// function aprobarMateria() {emit("aprobarMateria", materia.id);}

defineProps({
  materia: Object,
});
</script>

<style scoped>
tr:hover {
  background-color: lightgrey;
}
tr.apro {
  background-color: #a5d6a7;
  text-decoration: line-through;
}

tr.regu {
  background-color: #80deea;
}
.inhabilitado {
  background-color: gray !important;
  color: white;
}
.inhabilitado:hover {
  cursor: not-allowed;
}
</style>
>
