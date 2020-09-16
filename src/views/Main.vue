<template>
  <div class="main">
    <ul>
      <li v-for="movie of movies" v-bind:key="movie.id">
        {{movie.title}}
        <img :src="movie.image">
      </li>
    </ul>
  </div>
</template>

<script>
// @ is an alias to /src


export default {
  name: 'Main',
  data: function(){
    return {
      movies: []
    }
  },
  created: function(){
    const {token, URL} = this.$route.query

    fetch(`${URL}api/movies/`, {
      method: 'get',
      headers: {
        authorization: `jwt ${token}`
      }
    })
    .then(response => response.json())
    .then(data => {
      this.movies = data
      console.log(data)
    })
  }
}
</script>
