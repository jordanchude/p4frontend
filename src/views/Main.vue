<template>
  <div class="main">
    <div class="forms">
      <b-field label="Image"
          message="Enter an image URL">
          <b-input type="text" v-model="image"></b-input>
      </b-field>
      <b-field label="Title"
          message="Enter a movie title">
          <b-input type="text" v-model="title"></b-input>
      </b-field>
      <b-field label="Description"
          message="Enter a movie description">
          <b-input type="text" v-model="description"></b-input>
      </b-field>
      <b-button type="is-danger" @click="newMovie">Submit</b-button>
      <b-button type="is-danger" v-bind:id="editid" @click="updateMovie">Edit</b-button>
    </div>

    <ul>
      <li v-for="movie of movies" v-bind:key="movie.id">
        <p>{{movie.title}}</p>
        <img class="movie-images" :src="movie.image">
        <br/>
        <button class="button is-success is-outlined" v-bind:id="movie.id" @click="deleteMovie">Delete</button>
        <button class="button is-success is-outlined" v-bind:id="movie.id" @click="editMovieSelect">Edit</button>
        <router-link :to="{name: 'Movie', query: {URL: $route.query.URL, id: movie.id, token: $route.query.token}}" v-bind:URL="URL"><button class="button is-success is-outlined" v-bind:id="movie.id">Watch</button></router-link>
      </li>
    </ul>
  </div>
</template>

<script>
// @ is an alias to /src


export default {
  // <router-link :to="'/' + movie.id"> <button class="button is-success is-outlined" v-bind:id="movie.id">Watch</button> </router-link>
  name: 'Main',
  data: function(){
    return {
      movies: [],
      image: '',
      title: '',
      description: '',
      editImage: '',
      editTitle: '',
      editDescription: '',
      editid: null,
      URL: this.$route.query.URL,
      token: ''
    }
  },
  created: function(){
    this.token = window.localStorage.getItem('token')
    this.getMovies()
  },
  beforeMount: function() {
    // this.token = window.localStorage.getItem('token')
    // this.getMovies()

  },
  methods: {
    newMovie: function(){
      const { token, URL } = this.$route.query;

      fetch(`${URL}api/movies/`, {
        method: "post",
        headers: {
          authorization: `jwt ${token}`,
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ 
          image: this.image,
          title: this.title,
          description: this.description
          }),
      }).then(() => {
        this.image = ''
        this.title = ''
        this.description = ''
        this.getMovies();
      });
    },
  getMovies: function(){
    const {URL} = this.$route.query
    console.log(this.token)

    fetch(`${URL}api/movies/`, {
      method: 'get',
      headers: {
        Authorization: `jwt ${this.token}`,
        "Content-Type": "application/json"
      }
    })
    .then(response => response.json())
    .then(data => {
      this.movies = data
    })
  },
  editMovieSelect: function(event){
      const editid = event.target.id
      this.editid = editid
      const {token, URL} = this.$route.query
    
      fetch(`${URL}api/movies/${editid}/`, {
        method: "get",
        headers: {
          authorization: `jwt ${token}`,
          "Content-Type": "application/json"
        },
      })
      .then(response => response.json())
      .then(data => {
      this.image = data.image
      this.title = data.title
      this.description = data.description
    })
  },
  updateMovie: function() {
    const editid = this.editid
    const {token, URL} = this.$route.query

    fetch(`${URL}api/movies/${editid}/`, {
        method: "put",
        headers: {
          authorization: `jwt ${token}`,
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ 
          image: this.image,
          title: this.title,
          description: this.description
        }),
      })
      .then(() => {
        this.image = ''
        this.title = ''
        this.description = ''
        this.getMovies();
    });
  },
  deleteMovie: function(event){
  const id = event.target.id
  const {token, URL} = this.$route.query

  fetch(`${URL}api/movies/${id}/`, {
      method: "delete",
      headers: {
        authorization: `jwt ${token}`,
        "Content-Type": "application/json",
      },
    }).then(() => {
      this.getMovies();
    });
  }
  }
  }

</script>
<style>
.forms {
  width: 20%;
  margin: auto;
}

.movie-images {
  height: 373px;
}
</style>