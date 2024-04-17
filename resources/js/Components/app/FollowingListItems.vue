<script setup>
import FollowingItem from "@/Components/app/FollowingItem.vue";
import TextInput from "@/Components/TextInput.vue";
import { ref } from "vue";

const searchKeyword = ref("");
</script>

<template>
    <TextInput
        :model-value="searchKeyword"
        placeholder="Search for Friends"
        class="w-full mt-3"
    />
    <div class="mt-3 h-[400px] overflow-auto lg:flex-1">
        <div v-if="false" class="text-gray-400 text-center p-3">
            You are not following anyone at this time.
        </div>

        <div v-else>
            <FollowingItem
                image="https://randomuser.me/api/portraits/men/25.jpg"
                title="Bob Marley"
            />

            <FollowingItem
                image="https://randomuser.me/api/portraits/men/22.jpg"
                title="James Brown"
            />

            <FollowingItem
                image="https://randomuser.me/api/portraits/men/36.jpg"
                title="Rob Taylor"
            />

            <FollowingItem
                image="https://randomuser.me/api/portraits/women/36.jpg"
                title="Janet Green"
            />

            <FollowingItem
                image="https://randomuser.me/api/portraits/men/22.jpg"
                title="James Brown"
            />

            <FollowingItem
                image="https://randomuser.me/api/portraits/men/22.jpg"
                title="James Brown"
            />

            <FollowingItem
                image="https://randomuser.me/api/portraits/men/22.jpg"
                title="James Brown"
            />

            <FollowingItem
                image="https://randomuser.me/api/portraits/men/22.jpg"
                title="James Brown"
            />
        </div>
    </div>
</template>

<style scoped></style>
