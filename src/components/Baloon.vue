<template>
    <div class="baloon">
        <ul>
            <slot> </slot>
        </ul>
        <footer>
            <span class="arrow-down" />
        </footer>
    </div>
</template>

<script lang="ts" setup></script>

<style lang="scss" scoped>
.baloon {
    background: white;
    padding-inline: 5px;
    border-radius: 5px;
    box-shadow: 2px 2px 15px 1px rgba(53, 53, 53, 0.418);
    border: 1px solid #d3d3d3;
    position: relative;
    z-index: 9;

    button {
        /* background: red; */
        margin: 0;
        /* padding-block: 8px; */
        /* padding-inline: 12px; */
        height: 35px;
        width: 35px;
        border: none;
        font-size: 16px;
        background: transparent;
        cursor: pointer;
        z-index: 10;

        &:hover {
            background: #dddddd;
        }
    }

    footer {
        z-index: 9;
        position: absolute;
        top: 35px;
        left: 0;
        right: 0;
        display: flex;
        justify-content: center;
        /* background: red; */
        span.arrow-down {
            width: 0;
            height: 0;
            border-left: 8px solid transparent;
            border-right: 8px solid transparent;

            border-top: 10px solid white;
        }
    }
}
</style>
