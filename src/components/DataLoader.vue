<template>
    <div class="container-fluid">  <!-- Our project just needs Font Awesome solid + brand -->
        <div class="row d-flex flex-wrap justify-content-center gap-5 py-5">
            <div class="card col-5" style="width: 18rem;"  :key="data.id" v-for="data in queryData">
                <img :src="imgUrl + data.poster_path" :alt="data.name" class="card-img-top">
                <div class="card-body">
                    <h5 class="card-title">{{data.name}}</h5>
                    <p class="card-text">{{data.overview}}</p>
                </div>
                <ul class="list-group list-group-flush">
                    <li class="list-group-item">Popularity: {{data.popularity}}</li>
                    <li class="list-group-item">Vote count: {{data.popularity}} </li>
                    <li class="list-group-item d-flex align-items-center gap-1">
                        Vote average: <i class="fa fa-star" :key="index" v-for="(star, index) in parseInt(data.vote_average/2)"></i>
                    </li>
                </ul>
                <div class="card-body d-flex justify-content-between align-items-center">
                    <img width="50px" :src="getFlagIcon(data.original_language)" :alt="data.name">
                    <h6 class="text-secondary">{{data.release_date}}</h6>
                </div>
            </div>
        </div>
    
    </div>
</template>
<script>

export default{
    name : 'DataLoader',
    props : {
        queryMovieData : Array,
        queryTvData : Array,
    },
    data(){
        return {
            imgUrl : 'https://image.tmdb.org/t/p/w500',
            flagUrl : 'http://purecatamphetamine.github.io/country-flag-icons/3x2/'
        }
    },
    methods:{
        getFlagIcon(countryCode){
            if(countryCode === 'en')
                countryCode = 'gb'
            else if(countryCode === 'ja')
                countryCode = 'jm'
            return `${this.flagUrl+countryCode.toUpperCase()}.svg`
        },
        getStars(n){
            return parseInt(n/2)
        }
    },
    computed:{
        queryData(){
            const data = this.queryMovieData.concat(this.queryTvData)
            return data
        }
        
    }
    
}

</script>

<style scoped lang="sass">
    .card-body .card-text
        max-height: 200px
        overflow: auto
    img
        vertical-align: middle
    i
        color: gold
</style>