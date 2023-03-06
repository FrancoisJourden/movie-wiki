<template>
  <div>
    <div v-if="movie != null" class="flex flex-col flex-col-reverse sm:flex-row gap-2 m-2">
      <div class="p-2 rounded bg-gray-600 sm:basis-3/4 lg:basis-4/5">
        <p class="text-justify">{{ movie.overview }}</p>
        <br />
        <p>Genres: {{ movie.genres.map((genre) => genre.name).join(', ') }}</p>
        <p>Distribution: {{ movie.production_companies.map((genre) => genre.name).join(', ') }}</p>
        <br />
        <p>Release date: {{ new Date(movie.release_date).toLocaleDateString() }}</p>
        <br />
        <p class="flex">Average notation:
          <IconStarFilled v-for="index in Math.round(movie.vote_average/2)" :key="index"/>
          <IconStar v-for="index in 5-Math.round(movie.vote_average/2)" :key="index"/>
        </p>
        <p>Number of notations: {{ movie.vote_count }}</p>
      </div>
      <figure class="sm:basis-1/3 lg:basis-1/4 text-center">
        <img
          :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path"
          alt=""
          class="w-1/2 sm:w-full mx-auto rounded"
        />
        <figcaption>{{ movie.title }}</figcaption>
      </figure>
    </div>
    <div v-if="credits != null" class="grid grid-cols-5 lg:grid-cols-10 gap-2 p-2">
      <figure
        v-for="person in credits.cast.sort((a, b) => a.order > b.order).slice(0, 10)"
        :key="person"
      >
        <img
          :src="'https://image.tmdb.org/t/p/w500/' + person.profile_path"
          alt=""
          class="w-full mx-auto rounded my-auto"
        />
        <figcaption class="text-center mt-auto">{{ person.name }}</figcaption>
      </figure>
    </div>
  </div>
</template>

<script lang="ts">

import { IconStarFilled, IconStar } from "@tabler/icons-vue";

export default {
  components: { IconStarFilled, IconStar },
  data() {
    return {
      movie: null,
      credits: null
    }
  },
  methods: {
    async getMovieData(id: string) {
      const myUrl = new URL('https://api.themoviedb.org/3/movie/' + id)
      myUrl.searchParams.append('api_key', this.apiKey)
      myUrl.searchParams.append('language', 'en-US')

      const response = await fetch(myUrl.href)
      this.movie = await response.json()
    },
    async getMovieCredits(id: string) {
      const myUrl = new URL('https://api.themoviedb.org/3/movie/' + id + '/credits')
      myUrl.searchParams.append('api_key', this.apiKey)

      const response = await fetch(myUrl.href)
      this.credits = await response.json()
    }
  },
  beforeMount() {
    this.getMovieData(this.$route.params.id)
    this.getMovieCredits(this.$route.params.id)
  }
}
</script>

<style scoped></style>
