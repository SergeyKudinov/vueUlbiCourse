<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <div class="app_btns">
            <my-button
                @click="showDialog"
            >
                Создать пост
            </my-button>
            <MySelect
                v-model="selectedSort"
                :options="sortOptions"
            >
            </MySelect>
        </div>
        <my-dialog v-model:show="dialogVisible">
            <PostForm
                @create="createPost"
        />
        </my-dialog>
        <PostList
            :posts="posts"
            @remove="removePost"
            v-if="!isPostsLoading"
        />
        <div v-else>Идет загрузка...</div>
    </div>
</template>

<script>
    import PostList from './components/PostList.vue';
    import PostForm from './components/PostForm.vue';
    import axios from 'axios';
    import MySelect from './components/UI/MySelect.vue';

    export default {
        components: {
    PostList,
    PostForm,
    MySelect
},
        data() {
            return{
                posts: [],
                dialogVisible: false,
                isPostsLoading: false,
                selectedSort: '',
                sortOptions: [
                    {value: 'title', name: 'По названию'},
                    {value: 'body', name: 'По описанию'},
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
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
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
        }
    }
</script>

<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
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