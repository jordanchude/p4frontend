<template>
  <div class="main">
    <div>
      <h1>{{this.title}}</h1>
      <img :src="this.image">
      <p>{{this.description}}</p>
    </div>
    <div>
      <h1>WHERE TO WATCH</h1>
       <b-field label="Outlet"
          message="Enter an outlet">
          <b-input type="text" v-model="outlet"></b-input>
      </b-field>
      <b-field label="Link"
          message="Enter a link to view">
          <b-input type="text" v-model="link"></b-input>
      </b-field>
      <b-button type="is-danger" @click="createLink">Submit</b-button>
    <div v-for="link of links" v-bind:key="link.id">
        <a :href="link.link" target="_blank"><button>{{link.outlet}}</button></a>
        <button v-bind:id="link.id" @click="deleteLink">Delete</button>
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
  created: function(){
    this.token = window.localStorage.getItem('token')
    this.showMovie()
    this.getLinks()
  },
  methods: {
    showMovie: function(){
      const {URL, id} = this.$route.query
      fetch(`${URL}api/movies/${id}/`, {
        method: "get",
        headers: {
          authorization: `jwt ${this.token}`,
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
      const { URL } = this.$route.query;

      fetch(`${URL}api/links/`, {
        method: "post",
        headers: {
          authorization: `jwt ${this.token}`,
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
      const {URL, id} = this.$route.query
      console.log(`${URL}api/movies/${id}/links`)
      console.log(this.links)
      fetch(`${URL}api/movies/${id}/links`, {
        method: "get",
        headers: {
          authorization: `jwt ${this.token}`,
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
    const {URL} = this.$route.query
    fetch(`${URL}api/movies/${this.movie}/links/${id}`, {
        method: "delete",
        headers: {
          authorization: `jwt ${this.token}`,
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
</style>