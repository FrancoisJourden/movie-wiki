<script lang="ts">
import { IconStarFilled, IconStar } from '@tabler/icons-vue'
import { defineComponent } from 'vue'

export default defineComponent({
  components: { IconStarFilled, IconStar },
  data() {
    return {
      movie: null,
      credits: null,
      collection: null
    }
  },
  methods: {
    async getMovieData(id: string) {
      const movieDataUrl = new URL('https://api.themoviedb.org/3/movie/' + id)
      movieDataUrl.searchParams.append('api_key', this.api_key)
      movieDataUrl.searchParams.append('language', 'en-US')

      const response = await fetch(movieDataUrl.href)
      if (!response.ok) return
      this.movie = await response.json()
      if (this.movie.belongs_to_collection != null)
        await this.getMovieCollection(this.movie.belongs_to_collection.id)
      await this.getMovieCredits(this.movie.id)
    },
    async getMovieCredits(id: string) {
      const movieCreditsUrl = new URL('https://api.themoviedb.org/3/movie/' + id + '/credits')
      movieCreditsUrl.searchParams.append('api_key', this.api_key)

      const response = await fetch(movieCreditsUrl.href)
      if (!response.ok) return
      this.credits = await response.json()
    },
    async getMovieCollection(collection_id: string) {
      if (this.movie == null) return
      const movieCollectionUrl = new URL('https://api.themoviedb.org/3/collection/' + collection_id)
      movieCollectionUrl.searchParams.append('api_key', this.api_key)
      movieCollectionUrl.searchParams.append('language', 'en-US')

      const response = await fetch(movieCollectionUrl.href)
      if (!response.ok) return
      this.collection = await response.json()
    },
    loadData() {
      this.getMovieData(this.$route.params.id)
    }
  },
  beforeMount() {
    this.loadData()
    // this.getMovieCredits(this.$route.params.id)
  },
  watch: {
    '$route.params': 'loadData'
  }
})
</script>

<template>
  <div v-if="movie != null">
    <img
      :src="'https://image.tmdb.org/t/p/original/' + movie.backdrop_path"
      alt=""
      class="w-1/2 sm:w-full mx-auto rounded hidden sm:inline"
    />
    <div class="flex flex-col flex-col-reverse sm:flex-row gap-2 m-2">
      <div
        class="px-4 py-2 flex flex-col gap-2 rounded bg-gray-400 dark:bg-gray-600 sm:basis-3/4 lg:basis-4/5 2xl:basis-5/6"
      >
        <p class="text-justify">{{ movie.overview }}</p>
        <div class="my-auto flex flex-col gap-2">
          <div>
            <p>Genres: {{ movie.genres.map((genre) => genre.name).join(', ') }}</p>
            <p>
              Distribution: {{ movie.production_companies.map((genre) => genre.name).join(', ') }}
            </p>
          </div>
          <p>Release date: {{ new Date(movie.release_date).toLocaleDateString() }}</p>
          <div>
            <p class="flex">
              Average notation:
              <IconStarFilled v-for="index in Math.round(movie.vote_average / 2)" :key="index" />
              <IconStar v-for="index in 5 - Math.round(movie.vote_average / 2)" :key="index" />
            </p>
            <p>Number of notations: {{ movie.vote_count }}</p>
          </div>
          <div v-if="collection != null">
            <p>In the same collection:</p>
            <ul class="pl-4">
              <li
                v-for="collectionMovie in collection.parts.filter(
                  (myMovie) => myMovie.id !== this.movie.id
                )"
                :key="collectionMovie"
                class="hover:underline hover:cursor-pointer"
                @click="this.$router.push('/movie/' + collectionMovie.id)"
              >
                {{ collectionMovie.title }}
              </li>
            </ul>
          </div>
        </div>
      </div>
      <figure class="sm:basis-1/3 lg:basis-1/4 2xl:basis-1/6 text-center my-auto">
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
