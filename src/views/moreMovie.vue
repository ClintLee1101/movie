<template>
  <div class="movie-view">
    <header-bar @showTalion="open"></header-bar>
    <ScrollerNewMore title="免费下载观影" type="hasCover" :items="topMovies"></ScrollerNewMore>
    <talion-view v-show="talion" @closeTalion="close"></talion-view>
    <infinite-loading :on-infinite="onInfinite" ref="infiniteLoading">
      <loading slot="spinner"></loading>
    </infinite-loading>
  </div>
</template>

<script>
  import { mapState } from 'vuex'
  import ScrollerNewMore from '../components/ScrollerNewMore'
  import HeaderBar from '../components/HeaderBar'
  import TalionView from '../views/TalionView'

  import InfiniteLoading from 'vue-infinite-loading'
  import Loading from '../components/Loading'

  export default {
    name: 'movie-view',
    components: {HeaderBar, TalionView, ScrollerNewMore, InfiniteLoading, Loading},
    data () {
      return {
        talion: '',
        topMovies: [] // 热门高分免费电影
      }
    },
    computed: {
      // Getting Vuex State from store/modules/movie
      ...mapState({
//      topMovies: state => state.movie.topMovies,
      })
    },
    methods: {
      open: function () {
        this.talion = 'open'
      },
      close: function () {
        this.talion = ''
      },
      // Dispatching getMovie
      getMovie: function () {
        this.$store.dispatch('getMovie')
      },
      getIT: function (objectId) {
        this.$http.get('/api/movie/getMovie/')
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
      },
      onInfinite () {
        setTimeout(() => {
          this.goToMoreMovie()
          this.$refs.infiniteLoading.$emit('$InfiniteLoading:loaded')
        }, 1000)
      },
      goToMoreMovie: function () {
        console.log(this.topMovies)
        this.lastId = this.topMovies[this.topMovies.length - 1]._id
        let id = this.lastId
        var that = this
        this.$http.get(`/api/movie/getMovie2/${id}`)
          .then((response) => {
            // 响应成功回调
            if (response.body.length === 0) {
              this.$refs.infiniteLoading.$emit('$InfiniteLoading:complete')
            } else {
            }
            response.body.forEach(function (item) {
              that.topMovies.push(item)
            })
            console.log(that.topMovies)
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
      this.getMovie()
      this.getIT()
    }
  }
</script>

<style lang="scss" scoped>
  .hasCover {
    overflow-x: auto;
    white-space: nowrap;
    .title {
      display: block;
      max-width: 100%;
      margin-top: 1rem;
      line-height: 1.6rem;
      font-size: 1.6rem;
      color: #111;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      word-wrap: normal;
      text-align: center;
    }
  }
</style>
