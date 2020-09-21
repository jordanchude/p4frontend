<template>
  <div class="home">
    <h1 id="header-text">CINEFAVORITES</h1>
    <h2 id="subheader-text">All your favorite movies in one place.</h2>
    <video class="video" autoplay loop playsinline muted>
            <source src="./demo.mp4" type="video/mp4">
            Your browser does not support the video tag.
    </video>
    <b-carousel id="carousel" >
        <b-carousel-item v-for="(carousel, i) in carousels" :key="i">
            <section :class="`hero is-medium is-${carousel.color}`">
                <div class="hero-body has-text-centered">
                    <h1 class="title">{{carousel.text}}</h1>
                </div>
            </section>
        </b-carousel-item>
    </b-carousel>
  </div>
</template>

<script>
// @ is an alias to /src


export default {
  name: 'Home',
  components: {
    
  },
  data(){
        return {
            carousels: [
                { text: 'Slide 1', color: 'primary' },
                { text: 'Slide 2', color: 'info' },
                { text: 'Slide 3', color: 'success' },
                { text: 'Slide 4', color: 'warning' },
                { text: 'Slide 5', color: 'danger' }
            ]
        }
    }
}
</script>

<style>
  body {
    background: url(https://images.unsplash.com/photo-1574267432553-4b4628081c31?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2089&q=80);
    background-size: cover;
  }

  #header-text {
    font-size: 250%;
    color: white;
  }

  #subheader-text {
    color: white;
  }

  .video {
    width: 50%;
    min-width: 400px;
  }
  #carousel {
    width: 50%;
    min-width: 400px;
    margin: 0 auto;
  }

  @media 
</style>
