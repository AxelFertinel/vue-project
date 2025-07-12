h2
<template>
    <h2>Observateurs</h2>
    <Observateurs></Observateurs>
    <hr />

    <h2>onMounted/onUnmounted</h2>
    <button @click="showTimer = !showTimer">Afficher / Masquer</button>
    <p>
        <strong><Timer v-if="showTimer" /></strong>
    </p>
    <hr />

    <h2>Layout</h2>
    <Layout>
        <template #header>Entete</template>
        <template #aside>Aside</template>
        <template #main>Main</template>
        <template #footer>Footer</template>
    </Layout>
    <hr />

    <h2>todo liste</h2>
    <form action="" @submit.prevent="addTodo">
        <fieldset role="group">
            <input
                type="text"
                placeholder="Ajouter une tâche"
                v-model="newTodo"
                aria-label="Ajouter une tâche"
            />
            <button :disabled="newTodo.length === 0">Ajouter</button>
        </fieldset>
    </form>
    <h2>Liste des tâches</h2>
    <label>
        <input type="checkbox" v-model="hideCompleted" />Masquer les tâches
        complétées
    </label>
    <p v-if="remainingTodo > 0">
        Il reste {{ remainingTodo }} tâche{{ remainingTodo <= 1 ? "" : "s" }} à
        réaliser
    </p>
    <div v-if="todos.length === 0">
        <p>Aucune tâche pour l'instant</p>
    </div>
    <div v-else>
        <ul>
            <li
                :class="{ completed: todo.completed }"
                :key="todo.date"
                v-for="todo in sortedTodos"
            >
                <Checkbox
                    :label="todo.title"
                    :style="todo.completed"
                    @check="console.log('coché')"
                    @uncheck="console.log('decoché')"
                    v-model="todo.completed"
                />
            </li>
        </ul>
    </div>
</template>
<script setup>
import { computed, ref, onMounted } from "vue";
import Checkbox from "./components/Checkbox.vue";
import Timer from "./components/Timer.vue";
import Layout from "./components/Layout.vue";
import Observateurs from "./components/Observateurs.vue";

const showTimer = ref("true");
const newTodo = ref("");
const todos = ref([]);
onMounted(() => {
    fetch("https://jsonplaceholder.typicode.com/todos")
        .then((r) => r.json())
        .then(
            (v) => (todos.value = v.map((todo) => ({ ...todo, date: todo.id })))
        );
});

const addTodo = () => {
    todos.value.push({
        title: newTodo.value,
        completed: false,
        date: Date.now(),
    });
    newTodo.value = "";
};

const sortedTodos = computed(() => {
    const sortedTodos = todos.value.toSorted((a, b) =>
        a.completed > b.completed ? 1 : -1
    );

    if (hideCompleted.value === true) {
        return sortedTodos.filter((t) => t.completed === false);
    }

    return sortedTodos;
});

const hideCompleted = ref(false);
const remainingTodo = computed(() => {
    return todos.value.filter((t) => t.completed === false).length;
});
</script>
<style scoped>
.completed {
    opacity: 0.5;
    text-decoration: line-through;
}
</style>
