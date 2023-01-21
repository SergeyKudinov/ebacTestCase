<template>
    <div v-show="posts.length > 0">
        <h3>Список постов</h3>
        <transition-group name="post_list">
            <PostItem
                v-for="post in posts"
                :post="post"
                :key="post.id"
                @remove="$emit('remove', post)"
            />
        </transition-group>
    </div>
    <h2
        v-show="posts.length === 0"
        style="color: red">
        Список постов пуст
    </h2>
</template>

<script>
import PostItem from './PostItem.vue';

    export default {
        props: {
            posts: {
                type: Array,
                required: true
            }
        },
        components: { PostItem }
}
</script>

<style>
    .post_list-item {
        display: inline-block;
        margin-right: 10px;
    }
    .post_list-enter-active,
    .post_list-leave-active {
            transition: all 0.4s ease;
    }
    .post_list-enter-from,
    .post_list-leave-to {
            opacity: 0;
            transform: translateX(130px);
    }
    .post_list-move {
        transition: transform 0.4s ease;
    }
</style>