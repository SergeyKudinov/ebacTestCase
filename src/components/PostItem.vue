<template>
    <div class="post">
        <div>
            <div><strong>Title:</strong> {{post.title}}</div>
            <div><strong>Описание:</strong> {{post.body}}</div>
        </div>
        <div class="post_btns">
            <my-button
                @click="$emit('remove', post)"
            >
                Remove
            </my-button>
        </div>
    </div>
</template>

<script>
    import MyButton from './UI/MyButton.vue';

    export default {
        components: {
            MyButton,
        },
        props: {
            post: {
                type: Object,
                required: true,
            },
        },
    }
</script>

<style>
    .post {
        display: flex;
        justify-content: space-between;
        padding: 15px;
        border: 2px solid teal;
        margin-top: 10px;
    }
</style>