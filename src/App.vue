<template>
  <div id="app">
    <!--<Header @movies="setMovieData" @tvs="setTvData"></Header>-->
    <Header @data="setData"></Header>
    <!--<DataLoader :queryMovieData="movieData" :queryTvData="tvData"></DataLoader>-->
    <DataLoader :data="data"></DataLoader>
  </div>
</template>

<script>
import DataLoader from './components/DataLoader'
import Header from './components/Header'
export default {
  name: 'App',
  components: {
    DataLoader,
    Header,
  },
  data(){
    return {
      movieData : [],
      tvData : [],
      data: []
    }
  },
  methods : {
    setMovieData(data){
      this.movieData = data
    },
    setTvData(data){
      this.tvData = data
    },
    setData(data){
      this.data = data
    }
  }
}
</script>

<style lang="sass">
@import '~font-awesome/css/font-awesome.min.css'
@import '~bootstrap/scss/bootstrap'
@import './styles/variables'

#app
 background-color: #1b1b1b 
</style>
