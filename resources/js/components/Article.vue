<template>
    <div class="card">

        <div class="card-body">
            <div class="card-title">
                <h1> {{ article.title }}</h1>
            </div>
            <hr>
            <p> {{ article.body }}</p>
            <button class="btn btn-danger btn-sm col-md-12 mb-2" @click="$emit('delete-article',article)">Delete</button>
            <button class="btn btn-info btn-sm col-md-12" v-on:click="$emit('edit-article',article)">Edit</button>
        </div>
    </div>
</template>
<script>
    export default {
        name: 'Article',
        props:['article']
    }
</script>
<style scoped>

</style>
