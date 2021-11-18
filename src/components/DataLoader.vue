<template>
    <div>
        <div class="container-fluid">
            <div class="card-container row d-flex justify-content-start flex-nowrap py-5 px-0 overflow-auto">
                <span class="slide-controller-left" @click="scrollSlider('left')"> (:- </span>
                <div class="ratio-2x1 movie-card" :key="data.id" v-for="data in queryMovieData"
                 :style="{ backgroundImage: 'url(' + imgUrl + data.poster_path + ')' }">
                </div>
                <span class="slide-controller-right shadow" @click="scrollSlider('right')"> -:) </span>
            </div>
            <div class="card-container row d-flex justify-content-start flex-nowrap py-5 px-0 overflow-auto">
                <span class="slide-controller-left" @click="scrollSlider('left')"> (:- </span>
                <div class="ratio-2x1 movie-card" :key="data.id" v-for="data in queryTvData"
                 :style="{ backgroundImage: 'url(' + imgUrl + data.poster_path + ')' }">
                </div>
                <span class="slide-controller-right shadow" @click="scrollSlider('right')"> -:) </span>
            </div>
        </div>
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
            flagUrl : 'http://purecatamphetamine.github.io/country-flag-icons/3x2/',
            scrollCounter : 0
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
        },
        scrollSlider(direction){
            const scrollableContainer = document.querySelector('.card-container')
            if(direction === 'right'){
                this.scrollCounter++
                scrollableContainer.scrollLeft = window.innerWidth * this.scrollCounter
            }else{
                if(this.scrollCounter>0)
                    this.scrollCounter--
                
                scrollableContainer.scrollLeft = window.innerWidth * this.scrollCounter
            }
            
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
    @import './../styles/variables'
    .card-body .card-text
        max-height: 200px
        overflow: auto
    img
        vertical-align: middle
    i
        color: gold
    .ratio-2x1
        width: calc(100vw / 3)
        height: calc((100vw / 3) * 2) 
    .movie-card
        background-color: rgba(255,255,255,0.6)
    .card-container
        background-color: #1b1b1b
        scroll-behavior: smooth
        position: relative
        .slide-controller-right, .slide-controller-left
            position: sticky
            top: 50%
            transform: translateY(-50%)
            width: 60px
            height: 120px
            background-color: $bg-danger
            display: flex
            align-items: center
            justify-content: center
            color: white
            font-size: 32px
        .slide-controller-right
            right: 0
            border-top-left-radius: 15px
            border-bottom-left-radius: 15px
        .slide-controller-left
            left: 0
            border-top-right-radius: 15px
            border-bottom-right-radius: 15px
        .movie-card:first-of-type
            margin-left: -60px
        .movie-card:last-of-type
            margin-right: -60px
    
</style>