<template>
    <div class="post">
        <div>
            <div><strong>Название:</strong> {{post.title}}</div>
            <div><strong>Описание:</strong> {{post.body}}</div>
        </div>
        <div class="post_btns">
            <my-button
                @click="$emit('remove', post)"
            >
                Удалить
            </my-button>
        </div>
    </div>
</template>

<script>

export default {
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