<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <my-button @click="showDialog">
            Создать пост
        </my-button>
        <my-dialog v-model:show="dialogVisible">
            <PostForm
                @create="createPost"
        />
        </my-dialog>
        <PostList
            :posts="posts"
            @remove="removePost"
        />
    </div>
</template>

<script>
    import PostList from './components/PostList.vue';
    import PostForm from './components/PostForm.vue';

    export default {
        components: {
    PostList,
    PostForm,
},
        data() {
            return{
                posts: [
                    {id: 1, title: 'Javascript', body: 'Описание поста 1'},
                    {id: 2, title: 'Javascript 2', body: 'Описание поста 2'},
                    {id: 3, title: 'Javascript 3', body: 'Описание поста 3'},
                ],
                dialogVisible: false,
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
        },
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
    
</style>