<template>
  <div class="container">
    <h1>Welcome!</h1>
    <p><nuxt-link to="/TheProduct">Product page</nuxt-link></p>
    <p><nuxt-link to="/ContactPage">Contact page</nuxt-link></p>
  </div>
</template>

<script>
import { TweenMax, Back } from 'gsap'

export default {
  transition: {
    mode: 'out-in',
    css: false,
    beforeEnter (el) {
      TweenMax.set(el, {
        transformPerspective: 600,
        perspective: 300,
        transformStyle: 'preserve-3d'
      })
    },
    enter (el, done) {
      TweenMax.to(el, 1, {
        rotationY: 360,
        transformOrigin: '50% 50%',
        ease: Back.easeOut
      })
      done()
    },
    leave (el, done) {
      TweenMax.to(el, 1, {
        rotationY: 0,
        transformOrigin: '50% 50%',
        ease: Back.easeIn
      })
      done()
    }
  }
}
</script>

<style>
  html, body {
    font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
    background: #222;
    color: white;
    width: 100vw;
    height: 100vh;
  }
  a, a:visited {
    color: #3edada;
    text-decoration: none;
  }
  .container {
    padding: 60px;
    width: 100vw;
    height: 100vh;
    background: #444;
  }
</style>
