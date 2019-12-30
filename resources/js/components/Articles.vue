<template>
    <div class="articles">
        <h2>{{ totalArticle }} Articles</h2>
        <h1>{{this.pagination}}</h1>
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
        <ul class="pagination">
        <li class="page-item" v-if="pagination.prev" v-on:click.prevent="getArticles(pagination.prev)"><a class="page-link"  href="#">Previous</a></li>
        <li class="page-item" v-for="pnumb in pagination.total_page" v-on:click.prevent="getArticles(pagination.path+'?page='+pnumb)"><a class="page-link" href="#">{{ pnumb }}</a></li>
        <li class="page-item" v-if="pagination.next" v-on:click.prevent="getArticles(pagination.next.toString())"><a class="page-link" href="#">Next</a></li>
    </ul>
        <p v-for="article in articles">
            <Article v-bind:article="article" v-on:delete-article="deleteArticle(article)" v-on:edit-article="editArticle(article)" />
        </p>
    </div>
</template>
<script>
    import Article from "./Article";
    import Pagination from './Pagination'
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
                    edit:false,
                    totalArticle:0,
                    pagination: {
                        current_page:0,
                        total_page:0,
                        first_page:'',
                        prev:'',
                        next:'',
                        path:''
                    }
                }
                // "links": {
                //     "first": "http://localhost:8000/api/articles?page=1",
                //         "last": "http://localhost:8000/api/articles?page=4",
                //         "prev": null,
                //         "next": "http://localhost:8000/api/articles?page=2"
                // },
                // "meta": {
                //     "current_page": 1,
                //         "from": 1,
                //         "last_page": 4,
                //         "path": "http://localhost:8000/api/articles",
                //         "per_page": 15,
                //         "to": 15,
                //         "total": 54
                // }
        },
        name: 'articles',
        components: {
            Article,
            Pagination
        },
        methods:{
                test(ss){
                    alert(ss);
                },
            getArticles(page){
                if(page == null)
                    page='api/articles';
                axios.get(page)
                .then(data=>{
                    this.articles= data.data.data;
                    this.totalArticle=data.data.meta.total;
                    console.log(data);

                    this.pagination= {
                        current_page:data.data.meta.current_page,
                            total_page:data.data.meta.last_page,
                            first_page:data.data.links.first_page,
                            prev:data.data.links.prev,
                            next:data.data.links.next,
                            path:data.data.meta.path
                    }
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
    .dis{
        text-decoration: underline;
    }
</style>
