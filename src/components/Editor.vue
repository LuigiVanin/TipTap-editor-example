<script setup lang="ts">
import { useEditor, EditorContent, BubbleMenu } from "@tiptap/vue-3";
import StarterKit from "@tiptap/starter-kit";
import { onMounted, onUnmounted, ref } from "vue";
import BubbleMenuExt from "@tiptap/extension-bubble-menu";
import CopyIcon from "../assets/copy.svg";
import SaveIcon from "../assets/save.svg";

interface EditorProps {
    content: string;
    saveAction?: (content: string) => void;
}

const props = defineProps<EditorProps>();

const content = ref(props.content);
const editable = ref(false);
const saved = ref(false);

const save = () => {
    console.log("hapenning");
    turnOffEditor();
    props.saveAction && props.saveAction(content.value);
    // if (props.saveAction) {
    saved.value = true;
    // }

    console.log(saved.value);
};

const turnOffEditor = () => {
    editable.value = false;
    editor.value?.setEditable(false);
};

const turnOnEditor = () => {
    editable.value = true;
    editor.value?.setEditable(true);
    editor.value?.commands.focus();
};

const editor = useEditor({
    extensions: [StarterKit, BubbleMenuExt],
    content: content.value,
    editable: editable.value,
    injectCSS: false,
    onUpdate: (something) => {
        content.value = something.editor.getText();
    },
    onBlur: () => {
        save();
    },
});

onMounted(() => {
    content.value = editor.value?.getText() ?? "";
});

onUnmounted(() => {
    editor.value?.destroy();
});
</script>
<template>
    <div class="box">
        <header>
            <span class="saved" v-if="saved">
                <span>
                    <img :src="SaveIcon" alt="" />
                    salvo
                </span>
            </span>
        </header>
        <div
            :class="['editor-wrapper', editable || saved ? 'focus' : '']"
            @click="() => turnOnEditor()"
        >
            <BubbleMenu
                :editor="editor"
                :tippy-options="{ duration: 100 }"
                v-if="editor"
            >
                <button
                    @click="editor?.chain().focus().toggleBold().run()"
                    :class="{ 'is-active': editor.isActive('bold') }"
                >
                    bold
                </button>
                <button
                    @click="editor?.chain().focus().toggleItalic().run()"
                    :class="{ 'is-active': editor.isActive('italic') }"
                >
                    italic
                </button>
                <button
                    @click="editor?.chain().focus().toggleStrike().run()"
                    :class="{ 'is-active': editor.isActive('strike') }"
                >
                    strike
                </button>
                <button
                    @click="editor?.chain().focus().undo().run()"
                    :class="{ 'is-active': editor.isActive('undo') }"
                >
                    undo
                </button>
                <button
                    @click="editor?.chain().focus().redo().run()"
                    :class="{ 'is-active': editor.isActive('redo') }"
                >
                    redo
                </button>
            </BubbleMenu>
            <editor-content :editor="editor" style="padding-inline: 12px" />
            <footer>
                <button>
                    <img :src="CopyIcon" alt="" />
                </button>
                <button @click="saveAction && save()">
                    <img :src="SaveIcon" alt="" />
                </button>
            </footer>
        </div>
    </div>
</template>

<style lang="scss">
header {
    position: relative;
    height: 20px;
    z-index: 0;
    span {
        animation: saved-item 0.3s linear;
        width: 75px;
        height: 25px;
        position: absolute;
        right: 0px;
        background: #b199dc;
        border-radius: 2px;
        color: white;
        display: flex;
        align-items: start;
        justify-items: start;
        font-size: 14px;
        font-weight: 500;

        & > span {
            height: 20px;
            display: flex;
            align-items: center;
            justify-items: start;
            gap: 5px;
            padding-left: 4px;
        }
    }
}

.editor-wrapper {
    position: relative;
    z-index: 10;
    background: white;
    width: 100%;
    border-radius: 5px;
    text-align: start;
    border: 2px solid transparent;
    /* padding-inline: 16px; */
    padding-top: 12px;
    font-size: 14px;

    &.focus {
        border: 2px solid #b199dc;
    }

    p {
        padding-block: 0;
        margin-block: 0;
    }

    footer {
        display: flex;
        justify-content: end;
        padding-right: 4px;
        padding-bottom: 4px;
        gap: 4px;
        height: 29px;
        button {
            width: 24px;
            height: 25px;
            border: none;
            background: #b199dc;
            border-radius: 3px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.1s ease-in-out;

            &:hover {
                background: #41167f;
            }
        }
    }
}
/* .ProseMirror { */
/* min-height: 3rem; */
/* padding-inline: 16px !important; */
/* } */

.ProseMirror-focused {
    outline: none;
}

@keyframes saved-item {
    0% {
        top: 15px;
        opacity: 0;
    }

    100% {
        top: 0px;
        opacity: 1;
    }
}
</style>
