<script setup>
import { ref } from "vue";
const newTodo = ref("");
const todos = ref([
    {
        title: "Acheter la propriété 'Rue de la Paix'",
        completed: false,
        date: 1,
    },
    {
        title: "Construire un hôtel sur 'Avenue Foch'",
        completed: false,
        date: 2,
    },
    { title: "Éviter la case prison", completed: false, date: 3 },
]);

const addTodo = () => {
    todos.value.push({
        title: newTodo.value,
        completed: false,
        date: Date.now(),
    });
    newTodo.value = "";
};

const sortedTodos = () => {
    return todos.value.toSorted((a, b) => (a.completed > b.completed ? 1 : -1));
};
</script>

<template>
    <h1>todo liste</h1>

    <form action="" @submit.prevent="addTodo">
        <label for="task">Ajouter une tâche</label>
        <input type="text" placeholder="Ajouter une tâche" v-model="newTodo" />
        <button :disabled="newTodo.length === 0">Ajouter</button>
    </form>
    <h2>Liste des tâches</h2>
    <div v-if="todos.length === 0">
        <p>Aucune tâche pour l'instant</p>
    </div>
    <div v-else>
        <ul>
            <li
                :class="{ completed: todo.completed }"
                :key="todo.date"
                v-for="todo in sortedTodos()"
            >
                <label for=""
                    ><input
                        type="checkbox"
                        :style="todo.completed"
                        v-model="todo.completed"
                    />{{ todo.title }}</label
                >
            </li>
        </ul>
    </div>
</template>

<style scoped>
.completed {
    opacity: 0.5;
    text-decoration: line-through;
}
</style>
