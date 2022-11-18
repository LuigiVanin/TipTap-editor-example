<script setup lang="ts">
import { ref } from "vue";
import Editor from "./components/Editor.vue";
import Card from "./components/Card.vue";

const data = ref(
    "A Camisa Polo Lacoste é a escolha perfeita para um look formal ou casual. Sua originalidade e qualidade são incomparáveis. Além disso, ela é extremamente confortável! Seja para o trabalho, passeio ou lazer, a Camisa Polo Lacoste é ideal. Aproveite essa chance única de comprar uma peça exclusiva e de qualidade! Clique aqui para comprar agora mesmo."
);

const saving = async (content: string) => {
    console.log("saved that thing");
};

// const content = ref<string>("");
</script>

<template>
    <main>
        <Card>
            <Editor :content="data" />
        </Card>
    </main>
</template>

<style lang="scss" scoped>
main {
    width: 80%;
    margin-inline: auto;
}
</style>
