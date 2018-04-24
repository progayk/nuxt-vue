# Simple Server Side Rendering, Routing, and Page Transitions with Nuxt.js
Tutorial appears on: https://css-tricks.com/simple-server-side-rendering-routing-page-transitions-nuxt-js/

## Server Side Rendering
One particularly compelling aspect is the performance benefits. When we render our HTML, CSS, and JavaScript on the server, we often have less JavaScript to parse both initially and on subsequent updates. [This article](http://openmymind.net/2012/5/30/Client-Side-vs-Server-Side-Rendering/) does really well going into more depth on the subject:


**By rendering on the server, you can cache the final shape of your data.**

Instead of grabbing JSON or other information from the server, parsing it, then using JavaScript to create layouts of that information, we're doing a lot of those calculations upfront, and only sending down the actual HTML, CSS, and JavaScript that we need. This can reap a lot of benefits with caching, SEO, and speed up our apps and sites.

## What is Nuxt.js
Nuxt.js is a higher-level framework that you can use with a CLI command that you can use to create universal Vue applications. Here are some, not all, of the benefits:
* Server-Side Rendering
* Automatic Code Splitting
* Powerful Routing System
* Great lighthouse scores out of the gate 🐎
* Static File Serving
* ES6/ES7 Transpilation
* Hot reloading in Development
* Pre-processors: SASS, LESS, Stylus, etc
* Write Vue Files to create your pages and layouts!
* Easily add transitions to your pages

## Getting Set Up
If you don't have vue-cli install it via:
```bash
$ npm install -g vue-cli
```
Next, use the CLI to scaffold a new project, but we'll use Nuxt.js as the template:
```bash
vue init nuxt/starter my-project
cd my-project
yarn  # or...  npm install
npm run dev
```
You'll see the progress of the app being built and it will give you a dedicated development server to check out: http://127.0.0.1:3000/. This is what you'll see right away (with a pretty cool little animation):

![alt text](https://res.cloudinary.com/css-tricks/image/upload/c_scale,w_800,f_auto,q_auto/v1500213107/nuxt_b6lrda.gif "Logo Title Text 2")

Let's take a look at what's creating this initial view of our application at this point. We can go to the `pages` directory, and inside see that we have an `index.vue` page. If we open that up, we'll see all of the markup that it took to create that page. We'll also see that it's a `.vue` file, using single file components just like any ordinary `vue` file, with a template tag for the HTML, a script tag for our scripts, where we're importing a component, and some styles in a style tag. (If you aren't familiar with these, there's [more info on what those are here.](https://vuejs.org/v2/guide/single-file-components.html) The coolest part of this whole thing is that this `.vue` file doesn't require any special setup. It's placed in the `pages` directory, and Nuxt.js will automatically make this server-side rendered page!

Let's create a new page and set up some routing between them. In `pages/index.vue`, dump the content that's already there, and replace it.


<template>
  <div class="container">
    <h1>Welcome!</h1>
    <p><nuxt-link to="/product">Product page</nuxt-link></p>
  </div>
</template>

<style>
.container {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  padding: 60px;
}
</style>


Then let's create another page in the pages directory, we'll call it `product.vue` and put this content inside of it:


<template>
  <div class="container">
    <h1>This is the product page</h1>
    <p><nuxt-link to="/">Home page</nuxt-link></p>
  </div>
</template>


You might have noticed in here, there's a special little element: `<nuxt-link to="/">`. This tag can be used like an a tag, where it wraps around a bit of content, and it will set up an internal routing link between our pages. We'll use `to="/page-title-here"` instead of an href.


