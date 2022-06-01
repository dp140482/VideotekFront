<template>
  <div class="main-page">
    <FilmCarousel />
    <ListFilms header="Лучшие" :filmsList="ratingFilms" :key="getKey()" />
    <ListFilms header="Новинки" :filmsList="newestFilms" :key="getKey()" />
  </div>
</template>

<script>
import axios from 'axios'
import { host } from './../server/settings.js'
import { v4 as uuid } from 'uuid'
import FilmCarousel from './FilmCarousel.vue'
import ListFilms from './ListFilms.vue'

export default {
  name: 'MainPage',
  components: {
    FilmCarousel,
    ListFilms
  },
  data: () => ({
    newestFilms: null,
    ratingFilms: null
  }),
  methods: {
    getKey() { return uuid() }
  },
  created () {
    axios
      .post(host + '/get-films', { order: 'newest', arrayLength: 6 })
      .then( result => {
        console.log(result)
        this.newestFilms = result.data
        console.log(this.newestFilms)
      })
    axios
      .post(host + '/get-films', { order: 'rating', arrayLength: 6 })
      .then( result => {
        this.ratingFilms = result.data
      })
  }
}
</script>
