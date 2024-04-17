<script setup>
import TextInput from "@/Components/TextInput.vue";
import GroupItem from "@/Components/app/GroupItem.vue";

import { ref } from "vue";

const searchKeyword = ref("");
</script>

<template>
    <TextInput
        :model-value="searchKeyword"
        placeholder="Search for Groups"
        class="w-full mt-4"
    />
    <div class="mt-3 h-[400px] overflow-auto lg:flex-1">
        <div v-if="false" class="text-gray-400 text-center p-3">
            You are not joined to any groups
        </div>

        <div v-else>
            <GroupItem
                image="https://picsum.photos/100"
                title="Laravel Developers"
                description="Lorem ipsum, dolor sit amet consectetur adipisicing elit.
            Quaerat, ea. Error eligendi laborum in cupiditate accusantium,
            alias neque autem minima! Beatae fuga distinctio autem numquam
            error eos harum blanditiis quia?"
            />

            <GroupItem
                image="https://picsum.photos/100"
                title="Vue.js Developers"
                description="Lorem ipsum, dolor sit amet consectetur adipisicing elit.
            Quaerat, ea. Error eligendi laborum in cupiditate accusantium,
            alias neque autem minima! Beatae fuga distinctio autem numquam
            error eos harum blanditiis quia?"
            />
            <GroupItem
                image="https://picsum.photos/100"
                title="Vue.js Developers"
                description="Lorem ipsum, dolor sit amet consectetur adipisicing elit.
            Quaerat, ea. Error eligendi laborum in cupiditate accusantium,
            alias neque autem minima! Beatae fuga distinctio autem numquam
            error eos harum blanditiis quia?"
            />

            <GroupItem
                image="https://picsum.photos/100"
                title="Vue.js Developers"
                description="Lorem ipsum, dolor sit amet consectetur adipisicing elit.
            Quaerat, ea. Error eligendi laborum in cupiditate accusantium,
            alias neque autem minima! Beatae fuga distinctio autem numquam
            error eos harum blanditiis quia?"
            />
            <GroupItem
                image="https://picsum.photos/100"
                title="Vue.js Developers"
                description="Lorem ipsum, dolor sit amet consectetur adipisicing elit.
            Quaerat, ea. Error eligendi laborum in cupiditate accusantium,
            alias neque autem minima! Beatae fuga distinctio autem numquam
            error eos harum blanditiis quia?"
            />
            <GroupItem
                image="https://picsum.photos/100"
                title="Vue.js Developers"
                description="Lorem ipsum, dolor sit amet consectetur adipisicing elit.
            Quaerat, ea. Error eligendi laborum in cupiditate accusantium,
            alias neque autem minima! Beatae fuga distinctio autem numquam
            error eos harum blanditiis quia?"
            />
        </div>
    </div>
</template>
<style scoped></style>
