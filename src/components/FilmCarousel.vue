<template>
  <v-carousel cycle :show-arrows="false" height="60vh">
    <v-carousel-item v-for="(item, i) in carousel" :key="i">
      <router-link :to="{ name: 'filmPage', params: { route: item.route } }">
<!--        TODO заменить item.image на item.poster-->
        <img :src="getImgUrl(item.image)" class="carouselImg" alt="film"/>
      </router-link>
    </v-carousel-item>
  </v-carousel>
</template>

<script>
import {host} from '@/server/settings'
import axios from "axios";

export default {
  name: 'FilmCarousel',
  data: () => ({
    //TODO /* Перенести в базу данных */
    // carouselImages: [
    //   {
    //     src: 'sherlock.jpg'
    //   },
    //   {
    //     src: 'taboo.jpg'
    //   },
    //   {
    //     src: 'gentlemen.jpg'
    //   },
    //   {
    //     src: 'billions.jpg'
    //   },
    // ],
    carousel: []
  }),
  methods: {
    getImgUrl(img) {
      return host + '/images/' + img
    }
  },
  created() {
    axios
        .post(host + '/get-videocontent')
        .then(result => {
          const arr = result.data
          let n = 4
          let selection = new Array(n),
              len = arr.length,
              taken = new Array(len);
          if (n > len)
            throw new RangeError("getRandom: more elements taken than available");
          while (n--) {
            const x = Math.floor(Math.random() * len);
            selection[n] = arr[x in taken ? taken[x] : x];
            taken[x] = --len in taken ? taken[len] : len;
          }
          console.log(selection)
          this.carousel = selection
        })
  }
}
</script>

<style>
.carouselImg {
  width: 100%;
  height: 100%;
  object-fit: contain;
}
</style>
