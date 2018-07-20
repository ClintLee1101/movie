<template>
  <div class="movie-view has-header">
    <!--<ScrollerNew title="正在上映" type="hasCover" :items="hotMovies"></ScrollerNew>-->
    <ScrollerNew title="免费下载观影" type="hasCover" :items="topMovies"></ScrollerNew>
    <!--<scroller title="新片速递" type="hasCover" :items="newMovies"></scroller>-->
    <!--<scroller title="发现好电影" type="onlyString" :items="movieTags"></scroller>-->
    <!--<types></types>-->
    <!--<download-app></download-app>-->
  </div>
</template>

<script>
import { mapState } from 'vuex'

import Scroller from '../components/Scroller'
import ScrollerNew from '../components/ScrollerNew'
import Types from '../components/Types'
import DownloadApp from '../components/DownloadApp'

export default {
  name: 'movie-view',
  components: { Scroller, ScrollerNew, Types, DownloadApp },
  data () {
    return {
      topMovies: [], // 热门高分免费电影
      hotMovies: []
    }
  },
  computed: {
    // Getting Vuex State from store/modules/movie
    ...mapState({
//      hotMovies: state => state.movie.hotMovies,
//      topMovies: state => state.movie.topMovies,
      newMovies: state => state.movie.newMovies,
      movieTags: state => state.movie.movieTags
    })
  },
  methods: {
    // Dispatching getMovie
    getMovie: function () {
      this.$store.dispatch('getMovie')
    },
    getTopMovie: function (objectId) {
      const params1 = {objectId: objectId}
      this.$http.get('/api/movie/getTopMovies/', {params: params1})
        .then((response) => {
          // 响应成功回调
          this.hotMovies = response.body
        })
        .then((response) => {
          console.log(response)
        })
        .catch((reject) => {
          console.log(reject)
        })
    },
    getTopAccount: function (objectId) {
      const params1 = {objectId: objectId}
      this.$http.get('/api/login/getAccount/', {params: params1})
        .then((response) => {
          // 响应成功回调
//          this.hotMovies = response.body
        })
        .then((response) => {
          console.log(response)
        })
        .catch((reject) => {
          console.log(reject)
        })
    },
    getIT: function (objectId) {
      const params1 = {objectId: objectId}
      this.$http.get('/api/movie/getMovie/', {params: params1})
        .then((response) => {
          // 响应成功回调
          this.topMovies = response.body
        })
        .then((response) => {
          console.log(response)
        })
        .catch((reject) => {
          console.log(reject)
        })
    }
  },
  created () {
    // Getting movies data on created
    this.getMovie()
    this.getTopMovie()
    this.getIT()
    this.getTopAccount()
  }
}
</script>

<style scoped>

</style>
