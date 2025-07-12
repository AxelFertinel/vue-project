<template>
    <label>
        <input type="checkbox" @change="onChange" v-model="model" />
        {{ props.label }}
    </label>
</template>

<script setup>
const props = defineProps({
    label: String,
});
const model = defineModel();
const emits = defineEmits(["check", "uncheck"]);
const onChange = (event) => {
    if (event.currentTarget.checked) {
        emits("check");
    } else {
        emits("uncheck");
    }
};
</script>
