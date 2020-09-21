<template>
  <div class="main">
    <div>
      <h1 class="single-movie-title">{{this.title}}</h1>
      <img class="single-movie-image" :src="this.image">
      <p class="description">DESCRIPTION: {{this.description}}</p>
    </div>
    <div>
      <h1 id="where-to-watch">WHERE TO WATCH</h1>
        <b-collapse class="card" animation="slide" aria-id="contentIdForA11y3">
            <div
                slot="trigger" 
                class="card-header"
                role="button"
                aria-controls="contentIdForA11y3">
                <p class="card-header-title">
                    Add Links
                </p>
            </div>
            <div class="card-content">
                <div class="content">
                  <b-field label="Outlet" message="Enter an outlet">
                    <b-input class="single-movie-inputs" type="text" v-model="outlet"></b-input>
                  </b-field>
                  <b-field label="Link" message="Enter a link to view">
                    <b-input type="text" class="single-movie-inputs" v-model="link"></b-input>
                  </b-field>
                  <b-button type="is-primary" @click="createLink">Submit</b-button>
                </div>
            </div>
        </b-collapse>
      <div v-for="link of links" v-bind:key="link.id" class="watch-links">
        <a :href="link.link" target="_blank"><button class="is-success button is-medium">{{link.outlet}}</button></a><br/>
        <button v-bind:id="link.id" @click="deleteLink" class="delete-button button is-danger is-small" >Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'Movie',
  data: function(){
    return {
      image: '',
      title: '',
      description: '',
      movie: this.$route.query.id,
      outlet: '',
      link: '',
      links: [],
      token: ''
    }
  },
  created: function() {
    this.showMovie()
    this.getLinks()
  },
  methods: {
    showMovie: function(){
      const {URL, id, token} = this.$route.query
      fetch(`${URL}api/movies/${id}/`, {
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
    createLink: function(){
      const { URL, token} = this.$route.query;

      fetch(`${URL}api/links/`, {
        method: "post",
        headers: {
          authorization: `jwt ${token}`,
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ 
          movie: this.movie,
          outlet: this.outlet,
          link: this.link,
          is_public: true
          }),
      }).then(() => {
        this.outlet = ''
        this.link = ''
        this.getLinks();
      });
    },
    getLinks: function(){
      const {URL, id, token} = this.$route.query
      console.log(`${URL}api/movies/${id}/links`)
      console.log(this.links)
      fetch(`${URL}api/movies/${id}/links`, {
        method: "get",
        headers: {
          authorization: `jwt ${token}`,
          "Content-Type": "application/json"
        },
      })
      .then(response => response.json())
      .then(data => {
        this.links = data
      })
    },
    deleteLink: function(event){
    const id = event.target.id
    const {URL, token} = this.$route.query
    fetch(`${URL}api/movies/${this.movie}/links/${id}`, {
        method: "delete",
        headers: {
          authorization: `jwt ${token}`,
          "Content-Type": "application/json",
        },
      }).then(() => {
        this.getLinks();
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

.single-movie-inputs {
  width: 300px;
  min-width: 100px;
  margin: 0 auto;
}

#where-to-watch {
  display: inline-block;
  font-size: 20px;
  margin-top: 20px;
  padding: 10px;
  border-radius: 10px;
  color: white;
}

.single-movie-title {
  font-size: 20px;
  text-transform: uppercase;
  color: white;
}

.card {
  margin-top: 10px;
  margin-bottom: 20px;
}

.card-header-title {
  font-size: 20px;
}

.watch-links {
  display: inline-block;
  margin: 0px 10px 0px 10px;
}

.description {
  color: white;
}

.single-movie-image {
  width: 400px;
}
</style>