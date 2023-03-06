<script setup lang="ts">
import MovieCard from '@/components/MovieCard.vue'
import router from '@/router'
</script>
<script lang="ts">
export default {
  data() {
    return {
      movies: []
    }
  },
  methods: {
    async getMovies() {
      const myUrl = new URL('https://api.themoviedb.org/3/discover/movie')
      myUrl.searchParams.append('api_key', this.api_key)
      myUrl.searchParams.append('language', 'en-US')
      myUrl.searchParams.append('sort_by', 'popularity.desc')

      const response = await fetch(myUrl.href)
      const data = await response.json()
      this.movies = data.results
    }
  },
  beforeMount() {
    this.getMovies()
  }
}
</script>

<template>
  <main
    class="p-2 grid gap-2 grid-cols-1 sm:grid-cols-2 md:grid-cols-4 lg:grid-cols-5 2xl:grid-cols-7"
  >
    <MovieCard
      v-for="movie in movies"
      :key="movie"
      :movie="movie"
      @click="router.push('/movie/' + movie.id)"
    />
  </main>
  <footer class="dark:bg-slate-700 p-2">
    Copyright {{ new Date().getFullYear() }} @ François Jourden
  </footer>
</template>
