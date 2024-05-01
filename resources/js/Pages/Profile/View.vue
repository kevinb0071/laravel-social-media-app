<script setup>
import { computed, ref, watch } from "vue";
import { TabGroup, TabList, Tab, TabPanels, TabPanel } from "@headlessui/vue";
import { useForm, usePage } from "@inertiajs/vue3";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import TabItem from "./Partials/TabItem.vue";
import Edit from "@/Pages/Profile/Edit.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import { XMarkIcon, CheckCircleIcon } from "@heroicons/vue/20/solid";

const showNotification = ref(true);
const imagesForm = useForm({
    avatar: null,
    cover: null,
});

const authUser = usePage().props.auth.user;
const isMyProfile = computed(() => authUser && authUser.id === props.user.id);

const coverImageSrc = ref("");
const props = defineProps({
    errors: Object,
    mustVerifyEmail: {
        type: Boolean,
    },
    status: {
        type: String,
    },
    user: {
        type: Object,
    },
});

function cancelCoverImage() {
    imagesForm.cover = null;
    coverImageSrc.value = null;
}

function submitCoverImage() {
    imagesForm.post(route("profile.updateCover"), {
        onSuccess: (user) => {
            cancelCoverImage();
            setTimeout(() => {
                showNotification.value = false;
            }, 3000);
        },
    });
}

function onCoverChange(event) {
    imagesForm.cover = event.target.files[0];
    if (imagesForm.cover) {
        const reader = new FileReader();
        reader.onload = () => {
            coverImageSrc.value = reader.result;
        };
        reader.readAsDataURL(imagesForm.cover);
    }
}
</script>
<template>
    <AuthenticatedLayout>
        <div
            v-show="showNotification && status === 'cover-image-update'"
            class="my-2 py-2 px-3 font-medium text-sm bg-emerald-500 text-white"
        >
            Your cover image has been updated
        </div>

        <div
            v-if="errors.cover"
            class="my-2 py-2 px-3 font-medium text-sm bg-red-500 text-white"
        >
            {{ errors.cover }}
        </div>
        <div class="max-w-[900px] mx-auto h-full overflow-auto">
            <div class="group relative bg-white">
                <img
                    :src="
                        coverImageSrc ||
                        user.cover_url ||
                        '/img/defaultCover.jpg'
                    "
                    alt=""
                    class="w-full h-[200px] object-cover"
                />
                <div class="absolute top-2 right-2">
                    <button
                        v-if="!coverImageSrc"
                        class="bg-gray-50 hover:bg-gray-100 text-gray-800 py-1 px-3 text-xs flex items-center opacity-0 group-hover:opacity-100"
                        @change="onCoverChange"
                    >
                        <svg
                            xmlns="http://www.w3.org/2000/svg"
                            fill="none"
                            viewBox="0 0 24 24"
                            stroke-width="1.5"
                            stroke="currentColor"
                            class="w-4 h-4 mr-2"
                        >
                            <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                d="M6.827 6.175A2.31 2.31 0 0 1 5.186 7.23c-.38.054-.757.112-1.134.175C2.999 7.58 2.25 8.507 2.25 9.574V18a2.25 2.25 0 0 0 2.25 2.25h15A2.25 2.25 0 0 0 21.75 18V9.574c0-1.067-.75-1.994-1.802-2.169a47.865 47.865 0 0 0-1.134-.175 2.31 2.31 0 0 1-1.64-1.055l-.822-1.316a2.192 2.192 0 0 0-1.736-1.039 48.774 48.774 0 0 0-5.232 0 2.192 2.192 0 0 0-1.736 1.039l-.821 1.316Z"
                            />
                            <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                d="M16.5 12.75a4.5 4.5 0 1 1-9 0 4.5 4.5 0 0 1 9 0ZM18.75 10.5h.008v.008h-.008V10.5Z"
                            />
                        </svg>
                        Update Cover Image
                        <input
                            type="file"
                            class="absolute left-0 top-0 bottom-0 right-0 cursor-pointer opacity-0"
                        />
                    </button>
                    <div
                        v-else
                        class="flex gap-2 opacity-0 bg-white p-2 group-hover:opacity-100"
                    >
                        <button
                            @click="cancelCoverImage"
                            class="bg-gray-50 hover:bg-gray-100 text-gray-800 py-1 px-3 tx-xs flex items-center"
                        >
                            <XMarkIcon class="h-3 w-3 mr-2" />
                            Cancel
                        </button>

                        <button
                            @click="submitCoverImage"
                            class="bg-gray-800 hover:bg-gray-900 text-gray-100 py-1 px-3 text-xs flex items-center"
                        >
                            <CheckCircleIcon class="h-3 w-3 mr-2" />
                            Submit
                        </button>
                    </div>
                </div>
                <div class="flex">
                    <img
                        src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSFufKaz_aSCpRujjKFjPGhtH0fGUVCa75SvQ&s"
                        class="ml-[48px] w-[150px] h-[150px] -mt-[64px]"
                    />
                    <div class="flex justify-between items-center flex-1 p-4">
                        <h2 class="font-bold text-lg">{{ authUser.name }}</h2>
                        <PrimaryButton v-if="isMyProfile">
                            <svg
                                xmlns="http://www.w3.org/2000/svg"
                                fill="none"
                                viewBox="0 0 24 24"
                                stroke-width="1.5"
                                stroke="currentColor"
                                class="w-4 h-4 mr-2"
                            >
                                <path
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L6.832 19.82a4.5 4.5 0 0 1-1.897 1.13l-2.685.8.8-2.685a4.5 4.5 0 0 1 1.13-1.897L16.863 4.487Zm0 0L19.5 7.125"
                                />
                            </svg>

                            Edit Profile
                        </PrimaryButton>
                    </div>
                </div>
            </div>

            <div class="border-t">
                <TabGroup>
                    <TabList class="flex bg-white">
                        <Tab
                            v-if="isMyProfile"
                            v-slot="{ selected }"
                            as="template"
                        >
                            <TabItem text="About" :selected="selected" />
                        </Tab>
                        <Tab v-slot="{ selected }" as="template">
                            <TabItem text="Posts" :selected="selected" />
                        </Tab>

                        <Tab v-slot="{ selected }" as="template">
                            <TabItem text="Followers" :selected="selected" />
                        </Tab>
                        <Tab v-slot="{ selected }" as="template">
                            <TabItem text="Following" :selected="selected" />
                        </Tab>
                        <Tab v-slot="{ selected }" as="template">
                            <TabItem text="Photos" :selected="selected" />
                        </Tab>
                    </TabList>

                    <TabPanels class="mt-2">
                        <TabPanel
                            v-if="isMyProfile"
                            key="about"
                            class="bg-white p-3 shadow"
                        >
                            <Edit
                                :must-verify-email="mustVerifyEmail"
                                :status="status"
                            />
                        </TabPanel>

                        <TabPanel key="posts" class="bg-white p-3 shadow">
                            Post Content
                        </TabPanel>

                        <TabPanel key="followers" class="bg-white p-3 shadow">
                            Followers
                        </TabPanel>

                        <TabPanel key="following" class="bg-white p-3 shadow">
                            Following
                        </TabPanel>

                        <TabPanel key="photos" class="bg-white p-3 shadow">
                            Photos
                        </TabPanel>
                    </TabPanels>
                </TabGroup>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
