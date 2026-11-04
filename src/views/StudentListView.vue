<template>
  <h1>Annuaire des étudiants</h1>

  <form @submit.prevent="ajouterEtudiant">
    <input v-model="newStudent.firstName" placeholder="Prénom" />
    <input v-model="newStudent.lastName" placeholder="Nom" />
    <input v-model="newStudent.filiere" placeholder="Filière" />
    <input v-model.number="newStudent.year" type="number" min="1" max="5" placeholder="Année" />
    <input v-model="newStudent.email" type="email" placeholder="Email" />
    <button type="submit">Ajouter</button>
  </form>

  <div class="toolbar">
    <input v-model="recherche" placeholder="Rechercher par nom ou prénom..." />
    <select v-model="tri">
      <option value="lastName">Trier par nom</option>
      <option value="filiere">Trier par filière</option>
    </select>
  </div>

  <StudentCard v-for="student in etudiantsAffiches" :key="student.id" :student="student" />
  <p v-if="etudiantsAffiches.length === 0">Aucun étudiant trouvé.</p>
</template>

<script>
import StudentCard from '../components/StudentCard.vue'
import { store } from '../store/students.js'

export default {
  name: 'StudentListView',
  components: { StudentCard },
  data() {
    return {
      store,
      recherche: '',
      tri: 'lastName',
      newStudent: { firstName: '', lastName: '', filiere: '', year: 1, email: '' }
    }
  },
  computed: {
    // 1) filtrer (nom ou prénom, insensible à la casse), 2) trier.
    // filter() construit déjà un nouveau tableau : le sort() qui suit
    // ne modifie donc pas store.students.
    etudiantsAffiches() {
      const texte = this.recherche.trim().toLowerCase()
      const filtres = this.store.students.filter(s =>
        s.firstName.toLowerCase().includes(texte) ||
        s.lastName.toLowerCase().includes(texte)
      )
      return filtres.sort((a, b) => a[this.tri].localeCompare(b[this.tri]))
    }
  },
  methods: {
    ajouterEtudiant() {
      const prenom = this.newStudent.firstName.trim()
      const nom = this.newStudent.lastName.trim()
      if (!prenom || !nom) return

      // nouvel id = plus grand id existant + 1
      const id = Math.max(0, ...this.store.students.map(s => s.id)) + 1

      this.store.students.push({
        id,
        firstName: prenom,
        lastName: nom,
        filiere: this.newStudent.filiere.trim(),
        year: this.newStudent.year,
        email: this.newStudent.email.trim()
      })

      // réinitialiser le formulaire
      this.newStudent = { firstName: '', lastName: '', filiere: '', year: 1, email: '' }
    }
  }
}
</script>
