<template>
    <div
        class="column card"
        :style="{ height: props.height, width: props.width }"
    >
        <slot> </slot>
    </div>
</template>

<script lang="ts" setup>
import { defineProps, withDefaults } from "vue";

interface CardProps {
    height?: string;
    width?: string;
}

const props = withDefaults(defineProps<CardProps>(), {
    height: "auto",
    width: "auto",
});
</script>

<style lang="scss" scoped>
.card {
    background: white;
    /* border-top: 5px solid #41167f; */
    /* margin-top: 5px; */
    --tw-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1),
        0 2px 4px -2px rgb(0 0 0 / 0.1);
    --tw-shadow-colored: 0 4px 6px -1px var(--tw-shadow-color),
        0 2px 4px -2px var(--tw-shadow-color);
    box-shadow: var(--tw-ring-offset-shadow, 0 0 #0000),
        var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow);
    /* padding-block: 2px; */

    box-shadow: 2px 2px 15px 1px rgba(66, 66, 66, 0.322);
    position: relative;
    padding: 15px;
}
</style>
