<template>
  <div class="home">
    <h1 id="header-text">CINEFAVORITES</h1>
    <h2 id="subheader-text">All your favorite movies in one place.</h2>
    <video class="video" autoplay loop playsinline muted>
            <source src="./demo.mp4">
            Your browser does not support the video tag.
    </video>
    <b-carousel
        :autoplay="false"
        with-carousel-list
        :indicator="false"
        :overlay="gallery"
        @click="switchGallery(true)" class="test">
        <b-carousel-item v-for="(item, i) in items" :key="i">
            <figure class="image">
                <img :src="item.image">
                <h1>WHOOOOP</h1>
            </figure>
        </b-carousel-item>
        <span v-if="gallery" @click="switchGallery(false)" class="modal-close is-large"/>
        <template slot="list" slot-scope="props">
            <b-carousel-list
                v-model="props.active"
                :data="items"
                v-bind="al"
                @switch="props.switch($event, false)"
                as-indicator />
        </template>
        <template slot="overlay">
            <div class="has-text-centered has-text-white">
                Hello i'am overlay!
            </div>
        </template>
    </b-carousel>
  </div>
</template>

<script>
// @ is an alias to /src


export default {
  name: 'Home',
  components: {
    
  },
  data() {
        return {
            gallery: false,
            al: {
                hasGrayscale: true,
                itemsToShow: 2,
                breakpoints: {
                    768: {
                        hasGrayscale: false,
                        itemsToShow: 4
                    },
                    960: {
                        hasGrayscale: true,
                        itemsToShow: 6
                    }
                }
            },
            items: [
                {
                    title: 'Slide 1',
                    image: 'https://res.cloudinary.com/dpjdvsigb/image/upload/v1600737726/project-4-assets/third-carousel-image_znrdxk.jpg'
                },
                {
                    title: 'Slide 2',
                    image: 'https://res.cloudinary.com/dpjdvsigb/image/upload/v1600732087/project-4-assets/first-carousel-image_gaewop.jpg'
                },
                {
                    title: 'Slide 3',
                    image: 'https://res.cloudinary.com/dpjdvsigb/image/upload/v1600737725/project-4-assets/forth-carousel-image_b5w1wa.jpg'
                },
                {
                    title: 'Slide 4',
                    image: 'https://res.cloudinary.com/dpjdvsigb/image/upload/v1600732099/project-4-assets/forth-carousel-image_plht0l.png'
                }
            ]
        }
    },
    methods: {
        switchGallery(value) {
            this.gallery = value
            if (value) {
                document.documentElement.classList.add('is-clipped')
            } else {
                document.documentElement.classList.remove('is-clipped')
            }
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

  .test {
    width: 50%;
    min-width: 400px;
    margin: 0 auto;
  }

  @media 
</style>
