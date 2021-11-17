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
                    <button class="btn btn-primary" type="button" @click="performCallAPI">Button</button>
                </div>
            </div>
        </div>
        <DataLoader :queryData="data"></DataLoader>
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
            url : 'https://api.themoviedb.org/3/search/tv',
            token : 'eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI0NDZiOTBiOTc3MzE4ZWE0YmYyNmZmNzVlMWIxZTcxNiIsInN1YiI6IjYxOTRkMWMwM2UwOWYzMDAyYzg3MWQ5ZCIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.fqwWcWyQxrFrPVPOJtZuQPUYcoZ4QEvT38ET-Aj5gs0',
            data : []
        }
    },
    methods : {
        performCallAPI(){
            axios.get(`${this.url}?language=it_IT&query=` + this.searchString, { 'headers': {
                        Authorization : `Bearer ${this.token}`, 
                        'Content-Type': 'application/json' } 
            }).then((response) => {
                console.log(response.data.results)
                this.data.push(...response.data.results)
            });
        }
    },
    mounted(){
        this.performCallAPI()
    }
}
</script>
