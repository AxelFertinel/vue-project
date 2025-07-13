<template>
    <div class="question">
        <h3>{{ question.question }}</h3>
        <ul>
            <li v-for="(choice, index) in randomChoicies" :key="choice">
                <Answer
                    :id="`answer-${index}`"
                    :disabled="hasAnswer"
                    :value="choice"
                    @change="onAnswer"
                    :correctAnswer="question.correct_answer"
                    v-model="answer"
                />
            </li>
        </ul>
        <!-- <button :disabled="!hasAnswer" @click="emits('answer', answer)">
            Question suivante
        </button> -->
    </div>
</template>
<script setup>
import { shuffleArray } from "@/pages/array";
import { ref, computed, onMounted, onUnmounted } from "vue";
import Answer from "./Answer.vue";
const props = defineProps({
    question: Object,
});
const answer = ref(null);
const emits = defineEmits(["answer"]);
const hasAnswer = computed(() => answer.value !== null);
const randomChoicies = computed(() => shuffleArray(props.question.choices));

const onAnswer = () => {
    clearTimeout(timer);
    timer = setTimeout(() => {
        emits("answer", answer.value);
    }, 1500);
};

let timer;
onMounted(() => {
    timer = setTimeout(() => {
        answer.value = "";
        onAnswer();
    }, 3000);
});

onUnmounted(() => {
    clearTimeout(timer);
});
</script>

<style>
.question button {
    margin-left: auto;
    display: block;
}
</style>
