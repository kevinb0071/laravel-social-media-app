<script setup>
import { computed, ref } from "vue";
import { TabGroup, TabList, Tab, TabPanels, TabPanel } from "@headlessui/vue";
import { usePage } from "@inertiajs/vue3";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import TabItem from "./Partials/TabItem.vue";
import Edit from "@/Pages/Profile/Edit.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";

const authUser = usePage().props.auth.user;
const isMyProfile = computed(() => authUser && authUser.id === props.user.id);

const props = defineProps({
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
</script>
<template>
    <AuthenticatedLayout>
        <div class="w-[900px] mx-auto h-full overflow-auto">
            <div class="relative bg-white">
                <img
                    src="https://www.prodraw.net/fb_cover/images/fb_cover_65.jpg"
                    alt=""
                    class="w-full h-[200px] object-cover"
                />

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
