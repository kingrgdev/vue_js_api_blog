<template>
    <div>
        <h1>Hello World!</h1>
        <nav aria-label="Page navigation example">
        <ul class="pagination">
            <li v-bind:class="[{ disabled: !pagination.prev_page_url}]" class="page-item">
                <a class="page-link" href="#" @click="fetchArticles(pagination.prev_page_url)">Previous</a>
            </li>

            <li class="page-item disabled">
                <a class="page-link" href="#">Page {{ pagination.current_page }} of {{ pagination.last_page }}</a>
            </li>

            <li v-bind:class="[{ disabled: !pagination.next_page_url}]" class="page-item">
                <a class="page-link" href="#" @click="fetchArticles(pagination.next_page_url)">Next</a>
            </li>
        </ul>
        </nav>
        <div class="mb-2" v-for="article in articles" v-bind:key="article.id">
            <div class="card" style="width: 18rem;">
            <img class="card-img-top" v-bind:src="article.photo" alt="Card image cap">
            <div class="card-body">
                <h5 class="card-title">{{ article.title }}</h5>
                <p class="card-text">{{ article.description }}</p>
            </div>
            <div class="card-body">
                <a href="#" class="card-link">Card link</a>
                <a href="#" class="card-link">Another link</a>
            </div>
            </div>
        </div>
        
    </div>
</template>


<script>
    export default {
        data () {
            return {
                articles: [],
                articles: {
                    id: '',
                    title: '',
                    body: '',
                    photo: ''

                },
                product_id: '',
                pagination: {},
                edit:false
            }
        },

        created() {
            this.fetchArticles();
        },

        methods: {
            fetchArticles (page_url) {
                let vm = this;
                page_url = page_url || 'http://192.168.1.115:902/api/articles'
                fetch(page_url)
                .then(res => res.json())
                .then( res=>{
                    // console.log(res.data);
                    // console.log(res);
                    this.articles = res.data;
                    vm.makePagination(res.current_page, res.first_page_url, res.next_page_url, res.last_page_url, res.prev_page_url, res.last_page);
                })
                .catch(err => console.log(err));
            },
            makePagination(current, first, next, last, prev, last_page) {
                let pagination = {
                    current_page : current,
                    first_page_url : first,
                    next_page_url : next,
                    last_page_url : last,
                    prev_page_url: prev,
                    last_page: last_page
                }

                this.pagination = pagination;
            }
        }
    }
</script>