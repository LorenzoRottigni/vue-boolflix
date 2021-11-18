<template>
    <main>
        <div class="container">
            <div class="row">
                <div class="col-8 offset-2  mt-5 d-grid gap-2">
                    <h1>Search movies</h1>
                    <div class="input-group mb-3">
                        <span class="input-group-text" id="basic-addon3">https://example.com/users/</span>
                        <input type="text" class="form-control" v-model="searchString" id="basic-url" aria-describedby="basic-addon3">
                    </div>
                    <button class="btn btn-primary" type="button" @click="performCallAPI('search','movie'); performCallAPI('search','tv')">Button</button>
                </div>
            </div>
        </div>
    </main>
</template>
<script>
import axios from 'axios'


export default {
    name : 'Main',
    data(){
        return {
            searchString : undefined,
            url : 'https://api.themoviedb.org/3/',
            token : 'eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI0NDZiOTBiOTc3MzE4ZWE0YmYyNmZmNzVlMWIxZTcxNiIsInN1YiI6IjYxOTRkMWMwM2UwOWYzMDAyYzg3MWQ5ZCIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.fqwWcWyQxrFrPVPOJtZuQPUYcoZ4QEvT38ET-Aj5gs0',
            movieDataApi : [],
            tvDataApi : []
        }
    },
    methods : {
        performCallAPI(type, category){
            let APIurl
            if(type === 'trending')
                APIurl = `${this.url}${type}/${category}/week`
            else
                APIurl = `${this.url}${type}/${category}?language=it_IT&query=${this.searchString}`
            axios.get(APIurl, { 'headers': {
                        Authorization : `Bearer ${this.token}`, 
                        'Content-Type': 'application/json' } 
            }).then((response) => {
                console.log(response.data.results)
                if(category === 'movie'){
                    this.movieDataApi = response.data.results
                    this.$emit('movies', this.movieDataApi)
                }else{
                    this.tvDataApi = response.data.results
                    this.$emit('tvs', this.tvDataApi)
                }
            });
        }
    },
    mounted(){
        this.performCallAPI('trending','tv')
        this.performCallAPI('trending','movie')
    }
}
</script>
<style scoped lang="sass">
    main
        background-color: #1b1b1b
        color: white
</style>