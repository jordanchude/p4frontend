<template>
  <div class="home">
    <h1 id="header-text">CINEFAVORITES</h1>
    <h2>All your favorite movies in one place.</h2>
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
  #header-text {
    font-size: 50px;
  }
  .video {
    width: 50%;
  }
  #carousel {
    width: 50%;
    margin: 0 auto;
  }
</style>
