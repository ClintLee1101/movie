<template>
  <div class="scroller">
    <div class="header">
      <h2>{{title}}</h2>
      <a @click="goToMoreMovie()">更多</a>
    </div>
    <div class="content">
      <slot name="promItem"></slot>
      <ul class="hasCover" v-if="type === 'hasCover'">
        <li v-for="item in items">
          <router-link :to="'subjectNew/' + item._id" append>
            <img v-if="item.imgUrl" :src="item.imgUrl" alt="">
            <span class="title">{{item.title}}</span>
            <rating v-if="item.rating" :rating="item.rating"></rating>
          </router-link>
        </li>
      </ul>
      <ul class="onlyString" v-if="type === 'onlyString'">
        <li v-for="item in items" style="border-color: #FFAC2D;">
          <a :href="item.href" v-if="!item.line" :style="{color: item.color}">{{item.title}}</a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import Rating from './Rating'

  export default {
    name: 'scrollerNew',
    props: ['title', 'type', 'items'],
    components: { Rating },
    data () {
      return {
      }
    },
    methods: {
      getIT: function (objectId) {
        const params1 = {objectId: '5a1783dcf503af20bfad2fba'}
        this.$http.get('/api/movie/getMovie', {params: params1})
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
      goToMoreMovie: function () {
        this.$router.push({path: '/MoreMovie'})
//        let id = this.$route.params.id
        let id = '5a178262f503af20bfad2fb8'
//        const params1 = {objectId: '5a1783dcf503af20bfad2fba'}
        this.$http.get(`/api/movie/getMovie2/${id}`)
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
    }
  }
</script>

<style lang="scss" scoped>
  .scroller {
    padding-top: 1rem;
  }

  .header {
    height: 2.6rem;
    line-height: 2.6rem;
    padding: 0 1.6rem;

    a {
      float: right;
      font-size: 1.44rem;
      &:last-child {
        color: #42bd56;
      }
    }

    h2 {
      display: inline-block;
    }
  }

  .content {
    box-sizing: content-box;

    ul {
      padding: 0.8rem 0;
    }
  }

  .hasCover {
    overflow-x: auto;
    white-space: nowrap;
    text-align: center;

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
    }

    li {
      display: inline-block;
      width: 10rem;
      margin-left: 1rem;
    }

    li:first-child {
      padding-left: 0.8rem;
    }

    img {
      height: 15rem;
    }
  }

  .onlyString {
    overflow-x: auto;
    white-space: nowrap;

    li {
      display: inline-block;
      margin: 0 0 0.8rem 1.6rem;
      font-size: 1.6rem;
      border: solid 0.1rem;
      border-radius: 0.4rem;
      vertical-align: middle;
    }

    a {
      height: 5rem;
      line-height: 5rem;
      padding: 0 2.4rem;
      letter-spacing: 0.16rem;
      overflow: auto;
      display: block;
      text-align: center;
    }

    li:empty {
      width: 100%;
      display: block;
      height: 0.1rem;
      border: 0;
      margin: 0;
    }
  }
</style>
