<script setup>

defineProps({
    image: {
        type: String,
    },
    title: {
        type: String,
    },
    description: {
        type: String,
    },
});
</script>

<template>
    <div class="mb-3 cursor-pointer hover:bg-gray-200">
      <div class="flex items-start gap-3 py-2 px-3">
        <img
            :src="image"
            alt=""
            class="w-[48px] rounded-full"
        />
        <div>
            <h3 class="font-black text-xl">
                 {{ title }}
            </h3>
            <div class="text-xs text-gray-500">
                {{  description }}
            </div>
        </div>
      </div>
    </div>
</template>
