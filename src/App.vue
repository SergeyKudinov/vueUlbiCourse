<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <my-input
            :model-value="searchQuery"
            placeholder="Поиск..."
        />
        <div class="app_btns">
            <my-button
                @click="showDialog"
            >
                Создать пост
            </my-button>
            <my-select
                v-model="selectedSort"
                @update:model-value="setSelectedSort"
                :options="sortOptions"
            />
        </div>
        <my-dialog v-model:show="dialogVisible">
            <PostForm
                @create="createPost"
            />
        </my-dialog>
        <PostList
            :posts="sortedAndSearchedPosts"
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
    import MyInput from './components/UI/MyInput.vue';
    import MyButton from './components/UI/MyButton.vue';

    export default {
        components: {
            MyInput,
            MySelect,
            MyButton,
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
    .app {
        padding: 20px;
    }
    .app_btns {
        margin: 15px 0;
        display: flex;
        justify-content: space-between;
    }
</style>