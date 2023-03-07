<template xmlns="http://www.w3.org/1999/html">
  <figure class="bg-gray-300 dark:bg-gray-700 p-2 rounded flex flex-col hover:cursor-pointer">
    <img
      :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path"
      alt=""
      class="my-auto rounded"
    />
    <figcaption class="text-center">{{ movie.title }}</figcaption>
  </figure>
</template>

<script>
export default {
  name: 'MovieCard',
  props: {
    movie: {
      type: Object
    }
  }
}
</script>

<style scoped></style>
