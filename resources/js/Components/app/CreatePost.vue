<script setup>
import { ref } from "vue";
import InputTextArea from "@/Components/InputTextArea.vue";
import { useForm, usePage } from "@inertiajs/vue3";
import PostModal from "@/Components/app/PostModal.vue";

const authUser = usePage().props.auth.user;
const showModal = ref(false);
const newPost = ref({
    id: null,
    body: "",
    user: authUser,
});

function showCreatePostModal() {
    showModal.value = true;
}
</script>

<template>
    <div class="p-4 rounded-lg border bg-white mb-3">
        <div
            @click="showCreatePostModal"
            class="py-2 px-3 border-2 mb-3 w-full border-gray-200 rounded-md text-gray-500"
        >
            Click here to create a new post
        </div>

        <PostModal :post="newPost" v-model="showModal" />
    </div>
</template>

<style scoped></style>
