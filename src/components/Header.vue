<template>
    <main class="py-3 mb-5">
        <div class="container">
            <div class="row d-flex justify-content-around">
                <div class="col-4 d-flex align-items-center">
                    <img src="https://fontmeme.com/permalink/211119/c072857f28ffed69c9266a55183f4c20.png" alt="boolflix">
                </div>
                <div class="col-8 d-flex flex-column align-items-center justify-content-center">
                    <ul class="list-unstyled w-100 gap-5 d-flex justify-content-center ap-3">
                        <li><a href="#trending-Movies-day">Movies of the day</a></li>
                        <li><a href="#trending-Series-day">Series of the day</a></li>
                        <li><a href="#trending-Movies-week">Best movies of the week</a></li>
                        <li><a href="#trending-Series-week">Best series of the week</a></li>
                    </ul>
                    <div class="input-group">
                        <button class="btn btn-outline-danger" type="button" 
                        @click="filterCallAPI()"><i class="fa fa-search"></i></button>
                        <input type="text" class="form-control" placeholder="search" v-model="searchString" aria-label="Example text with button addon" aria-describedby="button-addon1">
                    </div>
                    <div class="d-flex gap-5 mt-3">
                        <div class="form-check form-switch">
                            <input class="form-check-input bg-danger" type="checkbox" role="switch" id="movies-checkbox" @click="toggleMovieFilter" checked>
                            <label class="form-check-label" for="movies-checkbox">Movies</label>
                        </div>
                        <div class="form-check form-switch">
                            <input class="form-check-input bg-danger" type="checkbox" role="switch" id="series-checkbox" @click="toggleTvFilter" checked>
                            <label class="form-check-label" for="series-checkbox">Series</label>
                        </div>
                    </div>
                    
                </div>
            </div>
        </div>
    </main>
</template>

<script>

    import axios from 'axios'

    export default {
        name : 'Header',
        data(){
            return {
                searchString : '',
                url : 'https://api.themoviedb.org/3/',
                token : 'your token',
                movieDataApi : [],
                tvDataApi : [],
                data : [],
                tvFilter : true,
                movieFilter : true
            }
        },
        methods : {
            performCallAPI(type, category, frequence){
                let APIurl
                if(type === 'trending'){
                    APIurl = `${this.url}${type}/${category}/${frequence}`
                }else{
                    APIurl = `${this.url}${type}/${category}?language=it_IT&query=${this.searchString}`
                }

                axios.get(APIurl, { 'headers': {
                            Authorization : `Bearer ${this.token}`, 
                            'Content-Type': 'application/json' } 
                }).then((response) => {
                    if(category === 'tv')
                        category = 'Series'
                    else if(category === 'movie')
                        category = 'Movies'
                    const results = response.data.results
                    for (const key in results) {
                        console.log(results[key])
                        results[key].displayStatus = false;
                    }
                    this.data.unshift({
                        content : results,
                        type : type,
                        category : category,
                        queryString : this.searchString,
                        frequence : frequence,
                        scrollPosition : 0,
                        id : `${type}-${category}-${this.searchString}${frequence}`
                    })
                    this.$emit('data', this.data)
                });
            },
            toggleMovieFilter(){
                if(this.movieFilter)
                    this.movieFilter = false
                else
                    this.movieFilter = true
            },
            toggleTvFilter(){
                if(this.tvFilter)
                    this.tvFilter = false
                else
                    this.tvFilter = true
            },
            filterCallAPI(){
                if(this.tvFilter)
                    this.performCallAPI('search','tv','')
                if(this.movieFilter)
                    this.performCallAPI('search','movie',''); 
                
            }
        },
        mounted(){
            this.performCallAPI('trending','tv','week')
            this.performCallAPI('trending','movie','week')
            this.performCallAPI('trending','tv','day')
            this.performCallAPI('trending','movie','day')
        }
    }
</script>

<style scoped lang="sass">
    @import './../styles/variables'
    main
        background-color: #1b1b1b
        color: white
    a 
        white-space: nowrap
        color: $bg-danger
        &:hover
            color: grey
    .btn-outline-danger
        border-color: $bg-danger
        color: $bg-danger
        &:hover
            background-color: $bg-danger
            border-color: $bg-danger
            color: white
    input.bg-danger
        background-color: $bg-danger !important
        border-color: $bg-danger !important
    
</style>
