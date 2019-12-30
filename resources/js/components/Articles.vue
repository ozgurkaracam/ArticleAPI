<template>
    <div class="articles">
        <h2>Articles</h2>
        <form class="mb-3">
            <div class="form-group">
                <label for="exampleInputEmail1">Article title</label>
                <input v-model="article.title" type="text" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="Article Title">
                <small id="emailHelp" class="form-text text-muted">Type Article Title..</small>
            </div>
            <div class="form-group">
                <label for="articleBody">Article Body</label>
                <textarea v-model="article.body" name="articleBody" id="articleBody" cols="30" rows="10" class="form-control" placeholder="Article Text.."></textarea>
                <small class="form-text text-muted">Type something..</small>
            </div>
            <button type="submit" class="btn btn-primary btn-success col-md-12 mb-1" v-on:click.prevent="postArticle" >Add Article</button>
            <button type="submit" v-if="edit" class="btn btn-primary btn-info col-md-12" v-on:click.prevent="editedArticle" >Edit Article</button>
        </form>

        <p v-for="article in articles">
            <Article v-bind:article="article" v-on:delete-article="deleteArticle(article)" v-on:edit-article="editArticle(article)" />
        </p>
    </div>
</template>
<script>
    import Article from "./Article";
    const axios= require('axios');
    const vueScrollTo=require('vue-scrollto');
    export default {
            data() {
                return {
                    articles:[],
                    article:{
                        title:'',
                        body:''
                    },
                    article_id:null,
                    edit:false
                }
        },
        name: 'articles',
        components: {
            Article
        },
        methods:{
            getArticles(){
                axios.get('/api/articles')
                .then(data=>{
                    this.articles= data.data.data;
                    console.log(data);
                })
                .catch(err=>{
                    console.log(err);
                });
            },
            postArticle(){

                    axios.post('/api/article',{
                        'title': this.article.title,
                        'body':this.article.body
                    }).then(data=>{
                        this.getArticles();
                    }).catch(err=>{alert('ERROR...');}

                    );

            },
            deleteArticle(article){
                axios.delete(`/api/article/${article.id}`)
                .then(
                    res=>{
                        this.getArticles();
                    }
                )
                .catch();
            },
            editArticle(article){
                this.edit=true;
                this.article.title=article.title;
                this.article.body=article.body;
                this.article_id=article.id;
            },
            editedArticle(){
                axios.put('/api/article',{
                    'article_id': this.article_id,
                    'title':this.article.title,
                    'body':this.article.body
                }).then(res=>{
                    this.getArticles();
                    this.article.title="";
                    this.article.body="";
                    this.edit=false;
                }).catch();

            }
        },
        created() {
            this.getArticles();
        }
    }
</script>
<style scoped>
    h2{
        color: red;
    }
</style>
