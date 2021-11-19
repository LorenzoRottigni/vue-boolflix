<template>
  <div id="app">
    <!--<Main @movies="setMovieData" @tvs="setTvData"></Main>-->
    <Main @data="setData"></Main>
    <!--<DataLoader :queryMovieData="movieData" :queryTvData="tvData"></DataLoader>-->
    <DataLoader :data="data"></DataLoader>
  </div>
</template>

<script>
import DataLoader from './components/DataLoader'
import Main from './components/Main'
export default {
  name: 'App',
  components: {
    DataLoader,
    Main,
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
</style>
