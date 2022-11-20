<script setup lang="ts">
import { useEditor, EditorContent, BubbleMenu } from "@tiptap/vue-3";
import StarterKit from "@tiptap/starter-kit";
import { onMounted, onUnmounted, ref } from "vue";
import useClipBoard from "vue-clipboard3";
import BubbleMenuExt from "@tiptap/extension-bubble-menu";
import BulletList from "@tiptap/extension-bullet-list";
import OrderedList from "@tiptap/extension-ordered-list";
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

const editor = useEditor({
    extensions: [StarterKit, BubbleMenuExt, BulletList, OrderedList],
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

const { toClipboard } = useClipBoard();

const copy = async () => {
    try {
        await toClipboard(content.value);
        console.log("copied");
    } catch (err) {
        console.log(err);
    }
};

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
        <div :class="['editor-wrapper', editable || saved ? 'focus' : '']">
            <BubbleMenu
                :editor="editor"
                :tippy-options="{ duration: 100 }"
                v-if="editor"
            >
                <!-- <Baloon> -->
                <div class="baloon">
                    <button
                        @click="editor?.chain().focus().toggleBold().run()"
                        :class="{ 'is-active': editor.isActive('bold') }"
                        style="font-weight: bold"
                    >
                        B
                    </button>
                    <button
                        @click="editor?.chain().focus().toggleItalic().run()"
                        :class="{ 'is-active': editor.isActive('italic') }"
                        style="font-style: italic"
                    >
                        I
                    </button>
                    <button
                        @click="editor?.chain().focus().toggleStrike().run()"
                        :class="{ 'is-active': editor.isActive('strike') }"
                    >
                        S
                    </button>
                    <button
                        @click="
                            editor?.chain().focus().toggleBulletList().run()
                        "
                        :class="{ 'is-active': editor.isActive('bulletList') }"
                    >
                        L
                    </button>
                    <button
                        @click="
                            editor?.chain().focus().toggleOrderedList().run()
                        "
                        :class="{ 'is-active': editor.isActive('orderedList') }"
                    >
                        1
                    </button>
                    <div class="vertical-divisor" />
                    <button
                        @click="editor?.chain().focus().undo().run()"
                        :class="{ 'is-active': editor.isActive('undo') }"
                    >
                        U
                    </button>
                    <button
                        @click="editor?.chain().focus().redo().run()"
                        :class="{ 'is-active': editor.isActive('redo') }"
                    >
                        R
                    </button>
                    <footer>
                        <span class="arrow-down" />
                    </footer>
                </div>
                <!-- </Baloon> -->
            </BubbleMenu>
            <editor-content
                :editor="editor"
                style="padding-inline: 12px"
                @click="() => turnOnEditor()"
            />
            <footer>
                <button @click="copy()">
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
            z-index: 20;
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

.baloon {
    background: white;
    padding-inline: 5px;
    border-radius: 5px;
    box-shadow: 2px 2px 15px 1px rgba(53, 53, 53, 0.418);
    border: 1px solid #d3d3d3;
    position: relative;
    z-index: 9;
    display: flex;
    align-items: center;
    justify-content: center;

    .vertical-divisor {
        height: 25px;
        width: 1px;
        background: #d3d3d3;
    }

    button {
        /* background: red; */
        margin: 0;
        /* padding-block: 8px; */
        /* padding-inline: 12px; */
        height: 35px;
        width: 30px;
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
