<template>
  <div class="container-media">
    <v-tabs
      dark
      grow
      v-model = "tab"
    >
      <v-tabs-slider></v-tabs-slider>
      <v-tab><p class="text-tab">Новости</p></v-tab>
      <v-tab><p class="text-tab">Статьи</p></v-tab>
      <v-tab><p class="text-tab">Видео</p></v-tab>
      <v-tab-item class="tab-item">
        <div class="list-cardNews">
          <NewsCard
            v-for="item in this.news"
            :key="item.id"
            :image="item.image"
            :item="item"
          />
        </div>
      </v-tab-item>
      <v-tab-item class="tab-item">
        <div class="list-articles">
          
        </div>
      </v-tab-item>
      <v-tab-item class="tab-item">
        <div class="list-video">
          
        </div>
      </v-tab-item>
    </v-tabs>
    <div class="line-media">
      <hr class="line" />
    </div>
  </div>
</template>

<script>
import { host } from '@/server/settings'
import axios from 'axios'
import NewsCard from '../components/NewsCard.vue'


export default {
  name: 'Media',
  components: {
    NewsCard,
  },
  data: () => ({
    news: null,
    tab: 0,
    tabActive: 0
  }),
  methods: {
      setNewsOrArticles(payload) {
        window.sessionStorage.setItem('newsOrArticles', JSON.stringify(payload))
      },
      getNewsOrArticles() {
        return JSON.parse(window.sessionStorage.getItem('newsOrArticles'))
      }
  },
  created () {
    this.tab = +window.sessionStorage.getItem('MediaActiveTab')
    axios
    .get(host + "/get-news")
      .then(result => {
        console.log(result)
        this.news = result.data
        window.sessionStorage.setItem('news', JSON.stringify(this.news))
      })
  },
  beforeDestroy() {
    window.sessionStorage.setItem('MediaActiveTab', this.tab)
  }
}
</script>

<style scoped>
.container-media{
    margin: 0px auto;
}
.list-cardNews{
    width: 1140px;
    margin: 30px auto;
    display: grid;
    grid-template-columns: repeat(4,1fr);
    grid-template-rows: repeat(1fr);
    grid-auto-flow: dense;
    grid-gap:30px;
}
.list-cardNews div:first-child{
 grid-column: span 2;
}
.list-cardNews div:nth-child(5n+1){
 grid-column: span 2;
}
.list-cardNews div:last-child{
 grid-column: span 2;
}
.text-tab{
    font-size: 13pt;
}
.text-tab:hover{
    color:#EB5804;
    cursor: pointer;
}
.v-tabs-slider{
    color:#EB5804
}
.tab-item{
    background-color: black;
}
.list-articles{
    width: 1140px;
    margin: 30px auto;
    display: grid;
    grid-template-columns: repeat(4,1fr);
    grid-template-rows: repeat(1fr);
    grid-auto-flow: dense;
    grid-gap:30px;
}
.list-articles div:nth-child(1){
    grid-column: span 2;
}
.list-articles div:last-child{
 grid-column: span 2;
}
.list-video{
    width: 1140px;
    margin: 30px auto;
    display: grid;
    grid-template-columns: repeat(2,1fr);
    grid-auto-flow: dense;
    grid-gap:10px;
}
.line-media{
    margin: 0 auto;
    width: 1140px;
}
</style>
