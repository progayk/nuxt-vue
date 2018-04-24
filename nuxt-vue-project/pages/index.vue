<template>
  <div class="container">
    <h1>Welcome!</h1>
    <p><nuxt-link to="/TheProduct">Product page</nuxt-link></p>
    <p><nuxt-link to="/ContactPage">Contact page</nuxt-link></p>
  </div>
</template>

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
  /*these properties is assigned to elements automatically by VueJs*/
  .page-enter-active {
    animation: acrossIn .45s ease-out both;
  }
  .page-leave-active {
    animation: acrossOut .65s ease-in both;
  }
  /*.page-enter {*/
    /*opacity: 0;*/
    /*transform-origin: 50% 50%;*/
  /*}*/
  @keyframes acrossIn {
    0% {
      transform: translate3d(-100%, 0, 0);
    }
    100% {
      transform: translate3d(0, 0, 0);
    }
  }

  @keyframes acrossOut {
    0% {
      transform: translate3d(0, 0, 0);
    }
    100% {
      transform: translate3d(100%, 0, 0);
    }
  }
</style>
