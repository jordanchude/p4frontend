# Project Overview

## Project Link
- [Link](https://cinefavorites.netlify.app/#/)

## Project Schedule

|  Day | Deliverable | Status
|---|---| ---|
|Day 1| Project Description | Complete
|Day 1| Wireframes / Priority Matrix / Timeline `backend` and `frontend`| Complete
|Day 2| Working RestAPI | Complete
|Day 3| Core Application Structure (HTML, CSS, etc.) | Complete
|Day 4| MVP & Bug Fixes | Complete
|Day 5| Final Touches and Present | Incomplete

## Project Description

My final project is a movie database called 'Cinefavorites'. Users will be able to create a login and subsequently login, where they'll be able to add one of their favorite movies to a list of films. The user will be able to add a title, director, description, and image to each film. After adding the film, the dashboard will display the image and title of the film. Once the user clicks on the film, they'll see the image, title, and description of the film on a separate page. On the separate page, they'll be able to add the platforms that they can stream and/or download the film on. 

## Google Sheet

- [Link](https://docs.google.com/spreadsheets/d/1MiYUM5Rr0hr_9kbYVNgYzxu88jngsMA9udl1Ox-z7Vw/edit?usp=sharing)

## Wireframes

- [Login](https://www.dropbox.com/s/ha4zgmijlqfok33/login-wireframe.pdf?dl=0)
- [Dashboard](https://www.dropbox.com/s/o2ki0mi8nvyyvd4/page-1-wireframe.pdf?dl=0)
- [Movie](https://www.dropbox.com/s/dz22kukh5bn3xg4/page-2-wireframe.pdf?dl=0)

## Time/Priority Matrix 
- [MVP](https://res.cloudinary.com/dpjdvsigb/image/upload/v1600056081/project-4-assets/frontend-mvp_gpnrdv.jpg)
- [Post-MVP](https://res.cloudinary.com/dpjdvsigb/image/upload/v1600056081/project-4-assets/frontend-post-mvp_jm8cvp.jpg)

### MVP/PostMVP - 5min  

#### MVP (examples)

- Pull data from API and populate onto page
- Update frontend with triggered changes in database
- Interface fully responsive with desktop, mobile, and tablet
- Add, delete, and update movie titles on dashboard
- Add 'where to watch' buttons on movies page

#### PostMVP 

- Social media links in footer

## Functional Components

#### MVP
| Letter | Component | Priority | Estimated Time | Time Invested |
| --- | --- | :---: |  :---: | :---: |
| A | Netlify Deployment | H | 1hr | 1hr |
| B | Login HTML | H | 1hr | 1hr |
| C | Login CSS | H | 3hr | 2hr |
| D | Login Vue Implementation | H | 4hr | 6hr |
| E | Dashboard HTML | H | 1hr | 1hr |
| F | Dashboard CSS | H | 3hr | 2hr |
| G | Dashboard Vue Implementation | H | 4hr | 4hr |
| H | Movies HTML | H | 1hr | 1hr |
| I | Movies CSS | H | 3hr | 2hr |
| J | Movies Vue Implementation | H | 4hr | 7hr |
| K | Login Testing and Debugging | H | 4hr | 3hr |
| L | Dashboard Testing and Debugging | H | 4hr | 2hr |
| M | Movies Testing and Debugging | H | 4hr | 6hr |
| N | Vue Research | H | 4hr | 3hr |
| - | Total | - | 41hr | 41hr |

#### PostMVP
| Letter | Component | Priority | Estimated Time | Time Invested |
| --- | --- | :---: |  :---: | :---: |
| A | Footer Social Media HTML | M | 1hr | -hr |
| B | Footer Social Media CSS | L | 1hr | -hr |
| - | Total | - | 2hr | -hr |

## Additional Libraries
- [Vue](https://vuejs.org/)
- [Buefy](https://buefy.org/)

## Code Snippet
- The following methods are responsible for user login/logout routing and placing the user's token in local storage. Although a temporary fix (i.e. this will need to be replaced with a more secure method), it's the first time I've worked with browser storage. This code keeps the user logged in when the page refreshes and erases the token when the user logs out.

```
methods: {
    login: function(event) {
      this.loggedIn = true
      this.token = event
      this.$router.push({
        path: "Main",
        query: { token: this.token.token, URL: this.URL },
      })
      window.localStorage.setItem('token', this.token.token)
    },
    logout: function(){
      this.loggedIn = false
      this.tokens = ''
      this.$router.push('/')
      window.localStorage.removeItem('token')
    }
  },
  beforeMount: function() {
    if (window.localStorage.getItem('token')) {
      this.token = window.localStorage.getItem('token')
      this.loggedIn = true
    } else {
      this.$router.push('/')
    }
  }
```

## Issues and Resolutions
**ERROR**: ```<title>ProgrammingError at /auth/users/register/</title>```                               
**RESOLUTION**: The deployed database needed to be reset and migrations needed to be re-run.