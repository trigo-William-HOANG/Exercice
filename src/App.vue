<script setup>
import { ref, computed } from "vue";
let id = 0;
// Creation des variables dynamiques utiles à notre code
const newTask = ref("");
const firstname = ref("William");
const tasks = ref([]);
const hideCompleted = ref(false);

const delTask = (task) => {
  // Filtre le tableau de sorte en affichant tous les tasks qui ne sont pas le task choisit. 
  tasks.value = tasks.value.filter((t) => t !== task);
};
const addTask = () => {
  var today = new Date();
  var dd = today.getDate();

  var mm = today.getMonth()+1; 
  var yyyy = today.getFullYear();
  if(dd<10) 
  {
      dd='0'+dd;
  } 

  if(mm<10) 
  {
      mm='0'+mm;
  } 

  today = mm+'/'+dd+'/'+yyyy;
  // Ajout d'un task dans notre tableau
  tasks.value.push({
    id: id++,
    title: newTask.value,
    completed: false,
    date: today
  });
  // Vidage de la variable dynamique permettant à attribuer un nom à notre tache
  newTask.value = ''
};
const filteredTasks = computed(() => {
  // D'abord on trie nos taches en mettant en priorité les tasks completés dans l'ordre négatif vers positif (Si complété en bas sinon en haut)
  const filteredTasks = tasks.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1)
  // Filtre qui prend tous les tasks déjà complétés et effectue u ntrie
  if (hideCompleted.value === true){
    return filteredTasks.filter(t => t.completed === false)
  }
  return filteredTasks
});

</script>

<template>
  <h1>Bonjour {{ firstname }}</h1>
  <div>
    <form action="" @submit.prevent="addTask">
      <fieldset role="group">
        <input v-model="newTask" type="title" placeholder="Tâche à effectuer" />
        <button :disabled="newTask.length === 0">Ajouter</button>
      </fieldset>
    </form>
    <div v-if="tasks.length === 0">Vous n'avez pas de tâches à effectuer</div>
    <div v-else>
    <ul>
      <li v-for="task in filteredTasks" :key="task.id">
        <input type="checkbox" v-model="task.completed" />
        <span :class="{ completed: task.completed }"
          >{{ task.title }}</span
        >
        <span style="font-size: 15px;"> créer le {{ task.date }}</span>
        <button @click="delTask(task)">Supprimer</button>
      </li>
    </ul>
    </div>
    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? "Tout afficher" : "Tout masquer" }}
    </button>
  </div>
</template>

<style>
.completed {
  text-decoration: line-through;
  opacity: 50%;
}
</style>
<!-- test -->