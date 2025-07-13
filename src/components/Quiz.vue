<template>
    <div v-if="state === 'error'"><p>Impossible de charger le quiz</p></div>
    <div :aria-busy="state === loading">
        <QuizContainer
            
            :quiz="quiz"
            v-if="quiz"
        ></QuizContainer>
    </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import QuizContainer from "./QuizContainer.vue";

const quiz = ref(null);
const state = ref("loading");
onMounted(() => {
    fetch("/quiz.json")
        .then((r) => {
            if (r.ok) {
                return r.json();
            }
            throw new Error("Erreur de chargement");
        })
        .then((data) => {
            quiz.value = data;
            state.value = "idle";
        })
        .catch((e) => {
            state.value = "error";
        });
});
</script>
