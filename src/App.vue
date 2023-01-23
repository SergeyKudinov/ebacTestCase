<template>
    <div class="app">
        <h1>Todos page</h1>
        <my-input
            :model-value="searchQuery"
            placeholder="Search..."
        />
        <div class="app_btns">
            <my-button
                @click="showDialog"
            >
                Create todo
            </my-button>
        </div>
        <my-dialog v-model:show="dialogVisible">
            <PostForm
                @create="createPost"
            />
        </my-dialog>
        <ItemList
            :posts="sortedAndSearchedPosts"
            @remove="removePost"
            v-if="!isPostsLoading"
        />
        <div v-else>Идет загрузка...</div>
    </div>
</template>

<script>
    import ItemList from './components/ItemList.vue';
    import PostForm from './components/PostForm.vue';
    import axios from 'axios';

    export default {
        components: {
            ItemList,
            PostForm,
        },
        data() {
            return{
                posts: [],
                dialogVisible: false,
                isPostsLoading: false,
                selectedSort: '',
                searchQuery: '',
                sortOptions: [
                    {value: 'title', name: 'По названию'},
                    {value: 'body', name: 'По содержимому'},
                ]
            }
        },
        methods: {
            createPost(post) {
                this.posts.push(post);
                this.dialogVisible = false;
            },
            removePost(post) {
                this.posts = this.posts.filter(p => p.id !== post.id)
            },
            showDialog() {
                this.dialogVisible = true;
            },
            async fetchPosts() {
                try {
                    this.isPostsLoading = true;
                    const response = await axios.get('https://jsonplaceholder.typicode.com/todos');
                    this.posts = response.data;
                } catch(e) {
                    console.log(e);
                } finally {
                    this.isPostsLoading = false;
                }
            },
        },
        mounted() {
            this.fetchPosts();
        },
        computed: {
            sortedPosts() {
                return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localCompare(post2[this.selectedSort]))
            },
            sortedAndSearchedPosts() {
                return this.sortedPosts.filter(post => post.title.includes(this.searchQuery))
            }
        }
    }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  h1 {
    margin-bottom: 15px;
  }
  .app {
    padding: 20px;
  }
  .app_btns {
    margin: 15px 0;
    display: flex;
    justify-content: space-between;
  }
</style>
