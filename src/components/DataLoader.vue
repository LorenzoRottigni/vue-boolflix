<template>
    <div>
        <div class="container-fluid">
            <div :key="'card-container-' + index" v-for="(element, index) in data" 
            class="card-container row d-flex justify-content-start flex-nowrap px-0 mb-5 py-6 overflow-auto" :id="element.id">
            <h1>{{element.category}} <span class="text-white" v-if="element.queryString != 'null'">{{element.queryString}}</span> <span class="text-white" v-if="(element.frequence != '')">{{'of the ' + element.frequence}}</span></h1>
                <span class="slide-controller-left" @click="scrollSlider('left',index)"> <i class="fa fa-angle-double-left"></i> </span>
                
                <div class="ratio-2x1 movie-card d-flex flex-column justify-content-end p-0 overflow-auto"
                 :key="'card-' + i" v-for="(data, i) in data[index].content"
                 :style="{ backgroundImage: 'url(' + imgUrl + data.poster_path + ')' }"
                 @click="displayInfo(index, i)">
                    <div class="movie-card-body bg-dark h-75 text-white py-3 px-2 d-flex flex-column justify-content-around"
                     :class="(data.displayStatus) ? 'opacity-1' : 'opacity-0'">
                        <div class="movie-card-body-top flex-grow-1 overflow-auto ">
                            <h5 class="card-title">{{data.name}}</h5>
                            <p class="card-text">{{data.overview}}</p>
                        </div>
                        <ul class="list-group list-group-flush rounded my-3">
                            <li class="list-group-item">Popularity: {{data.popularity}}</li>
                            <li class="list-group-item">Vote count: {{data.popularity}} </li>
                            <li class="list-group-item d-flex align-items-center gap-1">
                                Vote average: <i class="fa fa-star" :key="index" v-for="(star, index) in parseInt(data.vote_average/2)"></i>
                                <i class="fa fa-star text-white" :key="index" v-for="(star, index) in (5 - parseInt(data.vote_average/2))"></i>
                            </li>
                        </ul>
                        <div class="d-flex justify-content-between">
                            <img width="50px" :src="getFlagIcon(data.original_language)" :alt="data.original_language + '-flag'">
                            <h6 class="text-secondary mt-3">{{data.release_date}}</h6>
                        </div>
                        
                    </div>
                </div>
                <span class="slide-controller-right shadow" @click="scrollSlider('right', index)"> <i class="fa fa-angle-double-right"></i> </span>
            </div>
            <!--
            <div class="card-container row d-flex justify-content-start flex-nowrap py-5 px-0 overflow-auto">
                <span class="slide-controller-left" @click="scrollSlider('left')"> (:- </span>
                <div class="ratio-2x1 movie-card" :key="data.id" v-for="data in queryTvData"
                 :style="{ backgroundImage: 'url(' + imgUrl + data.poster_path + ')' }">
                </div>
                <span class="slide-controller-right shadow" @click="scrollSlider('right')"> -:) </span>
            </div>-->
        </div>


        <!--
        <div class="container-fluid">  
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
        </div>-->
    </div>
    
</template>
<script>

export default{
    name : 'DataLoader',
    props : {
        queryMovieData : Array,
        queryTvData : Array,
        data : Array
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
        scrollSlider(direction, i){
            const scrollableContainer = document.querySelector(`#${this.data[i].id}`)
            if(direction === 'right'){
                this.data[i].scrollPosition++
                scrollableContainer.scrollLeft = window.innerWidth * this.data[i].scrollPosition
            }else{
                if(this.data[i].scrollPosition>0)
                    this.data[i].scrollPosition--
                scrollableContainer.scrollLeft = window.innerWidth * this.data[i].scrollPosition
            }
            
        },
        displayInfo(i, id){
            if(this.data[i].content[id].displayStatus){
                this.data[i].content[id].displayStatus = false
            }else{
                this.data[i].content[id].displayStatus = true
            }
        }
    }
    
}

</script>

<style scoped lang="sass">
    @import './../styles/variables'
    .py-6
        padding-top: 5rem
        padding-bottom: 1rem
    .card-body .card-text
        max-height: 200px
        overflow: auto
    img
        vertical-align: middle
    i[class="fa fa-star"]
        color: gold
    i[class="fa fa-angle-double-left"], i[class="fa fa-angle-double-right"]
        color: white
        font-size: 48px
    .ratio-2x1
        width: calc(100vw / 4)
        height: calc((100vw / 4) * 2) 
    .movie-card
        background-color: rgba(255,255,255,0.6)
        background-repeat: no-repeat
        background-size: cover
        border-top: solid 2px $bg-danger
        border-bottom: solid 2px $bg-danger
        .movie-card-body
            transition: 1s
        ul
            li
                background-color: $bg-danger
                color: white

    .card-container
        background-color: #1b1b1b
        scroll-behavior: smooth
        /* width */
        &::-webkit-scrollbar 
            width: 3px
        /* Track */
        &::-webkit-scrollbar-track
            background-color: transparent
        /* Handle */
        &::-webkit-scrollbar-thumb 
            background: $bg-danger
            border-radius: 15px
        /* Handle on hover */
        &::-webkit-scrollbar-thumb:hover
            background: lightgray
        position: relative
        h1
            position: absolute
            top: 0
            left: 10px
            color: $bg-danger
            z-index: 200
        .slide-controller-right, .slide-controller-left
            position: sticky
            top: 50%
            transform: translateY(-50%)
            width: 50px
            height: 100px
            background-color: $bg-danger
            display: flex
            align-items: center
            justify-content: center
            color: white
            font-size: 28px
            z-index: 99
            i
                transition: 0.5s
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
