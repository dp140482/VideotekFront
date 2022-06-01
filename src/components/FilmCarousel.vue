<template>
  <v-carousel cycle :show-arrows="false" height="60vh">
    <v-carousel-item v-for="(item, i) in carouselImages" :key="i"
      ><img :src="getImgUrl(item.src)" class="carouselImg" alt="film" />
    </v-carousel-item>
  </v-carousel>
</template>

<script>
import { host } from './../server/settings.js'

export default {
  name: 'FilmCarousel',
  data: () => ({
    /* Перенести в базу данных */
    carouselImages: [
      {
        src: 'sherlock.jpg'
      },
      {
        src: 'taboo.jpg'
      },
      {
        src: 'gentlemen.jpg'
      },
      {
        src: 'billions.jpg'
      }
    ]
  }),
  methods: {
    getImgUrl(img) {
      return host + '/images/' + img
    }
  }
}
</script>

<style>
.carouselImg {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.carousel {
  margin: 0 auto;
}
</style>
