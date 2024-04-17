<script setup>
import PostItem from "@/Components/app/PostItem.vue";

const post1 = {
    group: null,
    attachments: [
    {
        id: 1,
        name: 'test.png',
        url: 'https://picsum.photos/1000',
        mime: 'image/png'
    },
    {
        id: 2,
        name: 'test2.png',
        url: 'https://picsum.photos/1000',
        mime: 'image/png'
    },
    {
        id: 3,
        name: 'MyDocument.docx',
        url: '#',
        mime: 'application/msword'
    }
],
    user: {
        id: 1,
        avatar: 'https://randomuser.me/api/portraits/men/30.jpg',
        name: "Kevin Bonds"
    },
    body: `<p>Over 500+ professionally designed, fully responsive, expertly crafted component examples you can drop into your Tailwind projects and customize to your heart’s content.</p>
    <p>Visually-stunning, easy to customize site templates built with React and Next.js. The perfect starting point for your next project and the ultimate resource for learning how experts build real websites with Tailwind CSS.</p>`,
    created_at: '04-13-2024 2:30'
};
const post2 = {
    group: {
        id: 1,
        name: 'Laravel Developers',
    },
    user: {
        id: 2,
        avatar: 'https://randomuser.me/api/portraits/men/31.jpg',
        name: "Jamie Doe"
    },
    body: `<p>Over 500+ professionally designed, fully responsive, expertly crafted component examples you can drop into your Tailwind projects and customize to your heart’s content.</p>
    <p>Visually-stunning, easy to customize site templates built with React and Next.js. The perfect starting point for your next project and the ultimate resource for learning how experts build real websites with Tailwind CSS.</p>`,
    created_at: '04-14-2024 9:30'
};
</script>

<template>
    <div class="overflow-auto">
       <PostItem :post="post1"/>
       <PostItem :post="post2"/>
    </div>
</template>

<style scoped></style>
