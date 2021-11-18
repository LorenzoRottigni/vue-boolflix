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
                    <button class="btn btn-primary" type="button" @click="performCallAPI('movie'); performCallAPI('tv')">Button</button>
                </div>
            </div>
        </div>
        <DataLoader :queryMovieData="movieData" :queryTvData="tvData"></DataLoader>
    </main>
</template>
<script>
import DataLoader from './DataLoader'
import axios from 'axios'
export default {
  components: { DataLoader },
    name : 'Main',
    data(){
        return {
            searchString : undefined,
            url : 'https://api.themoviedb.org/3/search/',
            token : 'eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI0NDZiOTBiOTc3MzE4ZWE0YmYyNmZmNzVlMWIxZTcxNiIsInN1YiI6IjYxOTRkMWMwM2UwOWYzMDAyYzg3MWQ5ZCIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.fqwWcWyQxrFrPVPOJtZuQPUYcoZ4QEvT38ET-Aj5gs0',
            movieData : [],
            tvData : []
        }
    },
    methods : {
        performCallAPI(category){
            axios.get(`${this.url}${category}?language=it_IT&query=` + this.searchString, { 'headers': {
                        Authorization : `Bearer ${this.token}`, 
                        'Content-Type': 'application/json' } 
            }).then((response) => {
                console.log(response.data.results)
                if(category === 'movie')
                    this.movieData.push(...response.data.results)
                else
                    this.tvData.push(...response.data.results)
            });
        }
    }
}
</script>
