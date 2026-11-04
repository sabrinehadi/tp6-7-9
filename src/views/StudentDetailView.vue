<template>
  <div v-if="student">
    <h1>{{ student.firstName }} {{ student.lastName }}</h1>

    <p>
      <label>Filière :
        <input v-model="student.filiere" />
      </label>
    </p>
    <p>
      <label>Année :
        <input v-model.number="student.year" type="number" min="1" max="5" />
      </label>
    </p>
    <p>
      <label>Email :
        <input v-model="student.email" type="email" />
      </label>
    </p>
  </div>
  <div v-else>
    <p>Étudiant introuvable.</p>
  </div>

  <button @click="$router.back()">Retour</button>
</template>

<script>
import { store } from '../store/students.js'

export default {
  name: 'StudentDetailView',
  computed: {
    // $route.params.id est une chaîne, les id du store sont des nombres :
    // on convertit avant de comparer. find() renvoie une RÉFÉRENCE vers
    // l'objet du store, donc les v-model modifient bien la donnée source.
    student() {
      return store.students.find(s => s.id === Number(this.$route.params.id))
    }
  }
}
</script>
