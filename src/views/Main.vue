<template>
  <div class="main">
  <b-collapse class="card" animation="slide" aria-id="contentIdForA11y3">
            <div
                slot="trigger" 
                class="card-header"
                role="button"
                aria-controls="contentIdForA11y3">
                <p class="card-header-title">
                    Add Movie
                </p>
            </div>
            <div class="card-content">
                <div class="content">
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
                    <b-button type="is-danger" @click="newMovie" class="movie-buttons">Submit Movie</b-button>
                    <b-button type="is-danger" v-bind:id="editid" @click="updateMovie" class="movie-buttons">Submit Edit</b-button>
                  </div>
            </div>
        </b-collapse>
    <ul class="movies">
      <li v-for="movie of movies" v-bind:key="movie.id" class="single-movie">
        <p class="movie-title">{{movie.title}}</p>
        <img class="movie-images" :src="movie.image">
        <br/>
        <button class="button is-success is-outlined single-movie-buttons" v-bind:id="movie.id" @click="deleteMovie">Delete</button>
        <button class="button is-success is-outlined single-movie-buttons" v-bind:id="movie.id" @click="editMovieSelect">Edit</button>
        <router-link :to="{name: 'Movie', query: {URL: $route.query.URL, id: movie.id, token: $route.query.token}}" v-bind:URL="URL"><button class="button is-success is-outlined single-movie-buttons" v-bind:id="movie.id">Watch</button></router-link>
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
  created: function() {
    this.$route.query.token = window.localStorage.getItem('token')
    this.getMovies()
  },
  methods: {
    newMovie: function(){
      const { URL, token } = this.$route.query;

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
      })
      .then(response => {
          if (response.status == 400) {
            alert('Movie with that title already exists')
            response.json()
          } else {
            alert('Movie added')
            this.image = ''
            this.title = ''
            this.description = ''
            this.getMovies();
            return response.json()
          }
        })
    },
  getMovies: function(){
    const {URL, token} = this.$route.query

    fetch(`${URL}api/movies/`, {
      method: 'get',
      headers: {
        Authorization: `jwt ${token}`,
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
      const {URL, token} = this.$route.query
    
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
    const {URL, token} = this.$route.query

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
  const {URL, token} = this.$route.query

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
  margin: auto;
}

.movies {
  display: flex;
  margin-top: 30px;
  justify-content: center;
}

.movie-title {
  font-size: 25px;
}

.movie-buttons {
  margin: 10px;
}

.movie-images {
  height: 373px;
}

.single-movie-buttons {
  margin: 5px;
}

.card {
  width: 50%;
}

.single-movie {
  margin: 0 10px 0 10px;
}
</style>