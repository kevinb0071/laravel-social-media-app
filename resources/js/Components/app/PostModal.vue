<script setup>
import { computed, onMounted, reactive, ref, watch } from "vue";
import {
    TransitionRoot,
    TransitionChild,
    Dialog,
    DialogPanel,
    DialogTitle,
} from "@headlessui/vue";

import PostUserHeader from "@/Components/app/PostUserHeader.vue";
import {
    XMarkIcon,
    PaperClipIcon,
    BookmarkIcon,
} from "@heroicons/vue/24/solid";
import { useForm } from "@inertiajs/vue3";
import ClassicEditor from "@ckeditor/ckeditor5-build-classic";
import { isImage } from "@/helpers";

const props = defineProps({
    post: {
        type: Object,
        required: true,
    },
    modelValue: Boolean,
});
const form = useForm({
    id: null,
    body: "",
    attachments: [],
});
const show = computed({
    get: () => props.modelValue,
    set: (value) => emit("update:modelValue", value),
});

const attachmentFiles = ref([]);

const emit = defineEmits(["update:modelValue"]);

const editor = ClassicEditor;

const editorConfig = {
    toolbar: [
        "heading",
        "|",
        "bold",
        "italic",
        "|",
        "link",
        "|",
        "bulletedList",
        "numberedList",
        "|",
        "outdent",
        "indent",
        "|",
        "blockQuote",
    ],
};

watch(
    () => props.post,
    () => {
        form.id = props.post.id;
        form.body = props.post.body;
    }
);

function resetModal() {
    form.reset()
    attachmentFiles.value = []
}

function closeModal() {
    show.value = false
    resetModal()
}


function submit() {
    form.attachments = attachmentFiles.value.map(myFile => myFile.file)
    if (form.id) {
        form.put(route("post.update", props.post.id), {
            preserveScroll: true,
            onSuccess: () => {
               closeModal()
            },
        });
    } else {
        form.post(route("post.create"), {
            preserveScroll: true,
            onSuccess: () => {
               closeModal()
            },
        });
    }
}



async function onAttachmentChoose($event) {
    for (const file of $event.target.files) {
        const myFile = {
            file,
            url: await readFile(file),
        };

        attachmentFiles.value.push(myFile);
    }
    $event.target.value = null;
}

async function readFile(file) {
    return new Promise((res, rej) => {
        if (isImage(file)) {
            const reader = new FileReader();
            reader.onload = () => {
                res(reader.result);
            };
            reader.onerror = rej;
            reader.readAsDataURL(file);
        } else {
            res(null);
        }
    });
}

function removeFile(myFile) {
    attachmentFiles.value = attachmentFiles.value.filter((f) => f !== myFile);
}
</script>
<template>
    <teleport to="body">
        <TransitionRoot appear :show="show" as="template">
            <Dialog as="div" @close="closeModal" class="relative z-50">
                <TransitionChild
                    as="template"
                    enter="duration-300 ease-out"
                    enter-from="opacity-0"
                    enter-to="opacity-100"
                    leave="duration-200 ease-in"
                    leave-from="opacity-100"
                    leave-to="opacity-0"
                >
                    <div class="fixed inset-0 bg-black/25"></div>
                </TransitionChild>

                <div class="fixed inset-0 overflow-y-auto">
                    <div
                        class="flex min-h-full items-center justify-center p-4 text-center"
                    >
                        <TransitionChild
                            as="template"
                            enter="duration-300 ease-out"
                            enter-from="opacity-0 scale-95"
                            enter-to="opacity-100 scale-100"
                            leave="duration-200 ease-in"
                            leave-from="opacity-100 scale-100"
                            leave-to="opacity-0 scale-95"
                        >
                            <DialogPanel
                                class="w-full max-w-md transform overflow-hidden rounded bg-white text-left align-middle shadow-xl transition-all"
                            >
                                <DialogTitle
                                    as="h3"
                                    class="flex items-center justify-between py-3 px-4 font-medium bg-gray-100 text-gray-900"
                                >
                                    {{
                                        form.id ? "Update Post" : "Create Post"
                                    }}
                                    <button
                                        @click="show = false"
                                        class="w-8 h-8 rounded-full hover:bg-black/5 transition flex items-center justify-center"
                                    >
                                        <XMarkIcon class="w-4 h-4" />
                                    </button>
                                </DialogTitle>
                                <div class="p-4">
                                    <PostUserHeader
                                        :post="post"
                                        :show-time="false"
                                        class="mb-4"
                                    />
                                    <ckeditor
                                        :editor="editor"
                                        v-model="form.body"
                                        :config="editorConfig"
                                    ></ckeditor>
                                    <div
                                        class="grid gap-3 my-3" :class="[
                                            attachmentFiles.length == 1 ? 'grid-cols-1' : 'grid-cols-2'
                                        ]"
                                    >
                                        <template
                                            v-for="(myFile, ind) of attachmentFiles"
                                        >
                                            <div
                                                class="group aspect-square bg-blue-100 flex flex-col items-center justify-center text-gray-500 relative"
                                            >

                                                <button
                                                    @click="removeFile(myFile)"
                                                    class="absolute z-20 right-3 top-3 w-7 h-7 flex items-center justify-center bg-black/20 text-white rounded-full hover:bg-black/30"
                                                >
                                                    <XMarkIcon
                                                        class="h-5 w-5"
                                                    />
                                                </button>

                                                <img
                                                    v-if="isImage(myFile.file)"
                                                    :src="myFile.url"
                                                    class="object-contain aspect-square"
                                                />
                                                <template v-else>
                                                    <PaperClipIcon
                                                        class="w-10 h-10 mb-3"
                                                    />
                                                    <small
                                                        class="text-center"
                                                        >{{
                                                            myFile.file.name
                                                        }}</small
                                                    >
                                                </template>
                                            </div>
                                        </template>
                                    </div>
                                </div>

                                <div class="flex gap-2 py-3 px-4">
                                    <button
                                        type="button"
                                        class="relative flex items-center justify-center rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 w-full"
                                        @click="submit"
                                    >
                                        <PaperClipIcon class="w-4 h-4 mr-2" />
                                        Attach File
                                        <input
                                            @click.stop
                                            @change="onAttachmentChoose"
                                            type="file"
                                            multiple
                                            class="absolute left-0 top-0 right-0 bottom-0 opacity-0"
                                        />
                                    </button>
                                    <button
                                        type="button"
                                        class="flex items-center justify-center rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 w-full"
                                        @click="submit"
                                    >
                                        <BookmarkIcon class="w-4 h-4 mr-2" />
                                        Submit
                                    </button>
                                </div>
                            </DialogPanel>
                        </TransitionChild>
                    </div>
                </div>
            </Dialog>
        </TransitionRoot>
    </teleport>
</template>

<style scoped></style>
