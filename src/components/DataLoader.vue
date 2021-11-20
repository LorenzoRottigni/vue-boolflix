<template>
    <div>
        <div class="container-fluid">
            <div :key="'card-container-'+index" v-for="(element, index) in data" >
                <h1>{{element.category}} <span class="text-white" v-if="element.queryString != 'null'">{{element.queryString}}</span> <span class="text-white" v-if="(element.frequence != '')">{{'of the ' + element.frequence}}</span></h1>
                <div class="card-container row d-flex justify-content-start flex-nowrap px-0 mb-5 py-4 overflow-auto" :id="element.id">
                    <span class="slide-controller-left" @click="scrollSlider('left',index)"> <i class="fa fa-angle-double-left"></i> </span>
                    <div class="ratio-2x1 movie-card d-flex flex-column justify-content-end p-0 overflow-auto"
                    :key="'card-' + i" v-for="(data, i) in data[index].content"
                    :style="{ backgroundImage: 'url(' + imgUrl + data.poster_path + ')' }"
                    @click="displayInfo(index, i)">
                        <h4 class="text-white text-center w-100 rotated-title"><span v-if="data.title">{{data.title}}</span><span v-else-if="data.name">{{data.name}}</span></h4>
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
                                    Vote average: <i class="fa fa-star" :key="'user-star-'+index" v-for="(star, index) in parseInt(data.vote_average/2)"></i>
                                    <i class="fa fa-star text-white" :key="'system-star-'+index" v-for="(star, index) in (5 - parseInt(data.vote_average/2))"></i>
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
        position: relative
        .movie-card-body
            transition: 1s
        ul
            li
                background-color: $bg-danger
                color: white
        .rotated-title
            position: absolute
            top: 0
            left: 0
            z-index: 99
            padding: 1rem 0
            background-color: $bg-danger-rgba 
            border-bottom-right-radius: 10px
            border-bottom-left-radius: 10px
            font-size: 18px
    h1
        color: $bg-danger
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
            margin-left: -50px
        .movie-card:last-of-type
            margin-right: -50px
    
</style>
