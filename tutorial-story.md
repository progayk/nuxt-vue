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

```html
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
```

Then let's create another page in the pages directory, we'll call it `product.vue` and put this content inside of it:

```html
<template>
  <div class="container">
    <h1>This is the product page</h1>
    <p><nuxt-link to="/">Home page</nuxt-link></p>
  </div>
</template>
```



You might have noticed in here, there's a special little element: `<nuxt-link to="/">`. This tag can be used like an a tag, where it wraps around a bit of content, and it will set up an internal routing link between our pages. We'll use `to="/page-title-here"` instead of an href.

## Creating Page Transitions
We already have a really cool progress bar that runs across the top of the screen as we’re routing and makes the whole thing feel very zippy. (That’s a technical term). While I like it very much, it won’t really fit the direction we’re headed in, so let’s get rid of it for now.

We're going to go into our `nuxt.config.js` file and change the lines:

```javascript
/*
** Customize the progress-bar color
*/
loading: { color: '#3B8070' },
```
to

```javascript
loading: false,
```

You'll also notice a few other things in this nuxt.config.js file. You'll see our meta and head tags as well as the content that will be rendered inside of them. That's because we won't have a traditional `index.html` file as we do in our normal CLI build, Nuxt.js is going to parse and build our `index.vue` file together with these tags and then render the content for us, on the server. If you need to add CSS files, fonts, or the like, we would use this Nuxt.js config file to do so.

Create page transitions. In order to understand what’s happening on the page that we’re plugging into, we need to review how the transition component in Vue works. I've written [an article all about this here](https://css-tricks.com/intro-to-vue-5-animations/), so if you'd like deeper knowledge on the subject, you can check that out. But what you really need to know is this: under the hood, Nuxt.js will plug into the functionality of Vue's transition component, and gives us some defaults and hooks to work with:

![alt text](https://res.cloudinary.com/css-tricks/image/fetch/q_auto,f_auto/https://cdn.css-tricks.com/wp-content/uploads/2017/01/transition.png "vue transition")

You can see here that we have a hook for what we want to happen **right before** the animation starts `enter`, **during** the animation/transition `enter-active`, and when it **finishes**. We have these same hooks for when something is leaving, prepended with `leave` instead. We can make simple transitions that just interpolate between states, or we could plug a full CSS or JavaScript animation into them.

Usually in a Vue application, we would wrap a component or element in `<transition>` in order to use this slick little functionality, but Nuxt.js will provide this for us at the get-go. Our hook for the page will begin with, thankfully- page. All we have to do to create an animation between pages is add a bit of CSS that plugs into the hooks:

```css
.page-enter-active, .page-leave-active {
  transition: all .25s ease-out;
}
.page-enter, .page-leave-active {
  opacity: 0;
  transform-origin: 50% 50%;
}
```

I'm also going to add an extra bit of styling here so that you can see the page transitions a little easier:

```css
html, body {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  background: #222;
  color: white;
  width: 100vw;
  height: 100vh;
}
```
Right now we're using a CSS Transition. This only gives us the ability to designate what to do in the middle of two states. We could do something a little more interesting by having an animation adjust in a way that suggests where something is coming from and going to. For that to happen, we could separate out transitions for page-enter and page-leave-active classes, but it's a little more DRY to use a CSS animation and specify where things are coming from and going to, and plug into each for `.page-enter-active`, and `.page-leave-active`:

```css
.page-enter-active {
  animation: acrossIn .45s ease-out both;
} 

.page-leave-active {
  animation: acrossOut .65s ease-in both;
} 

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
```
Let's also add a little bit of special styling to the product page so we can see the difference between these two pages:


```html
<style scoped> 
  .container {
    background: #222;
  }
  </style>
```

Now, let's say we have a page with a totally different interaction. For this page, the movement up and down was too much, we just want a simple fade. For this case, we'd need to rename our transition hook to separate it out.

Let’s create another page, we’ll call it the contact page and create it in the pages directory.

```html
<template>
  <div class="container">
    <h1>This is the contact page</h1>
    <p><nuxt-link to="/">Home page</nuxt-link></p>
  </div>
</template>

<script>
export default {
  transition: 'fadeOpacity'
}
</script>

<style>
.fadeOpacity-enter-active, .fadeOpacity-leave-active {
  transition: opacity .35s ease-out;
}

.fadeOpacity-enter, .fadeOpacity-leave-active {
  opacity: 0;
}
</style>
```


You can see how we could build on these further and create more and more streamlined CSS animations per page.

## Javascript Hooks

Vue's `<transition>` component offers some hooks to use JavaScript animation in place of CSS as well. They are as follows, and each hook is optional. The `:css="false"` binding lets Vue know we're going to use JS for this animation:

```html
<transition 
  @before-enter="beforeEnter"
  @enter="enter"
  @after-enter="afterEnter"
  @enter-cancelled="enterCancelled"

  @before-Leave="beforeLeave"
  @leave="leave"
  @after-leave="afterLeave"
  @leave-cancelled="leaveCancelled"
  :css="false">
 
 </transition>

```

The other thing we have available to us are transition modes. I'm a big fan of these, as you can state that one animation will wait for the other animation to finish transitioning out before transitioning in. The transition mode we will work with will be called out-in.

We can do something really wild with JavaScript and the transition mode, again, we're going a little nuts here for the purposes of demo, we would usually do something much more subtle:

<iframe width="560" height="315" src="https://www.youtube.com/embed/8t1PdiziI_U" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

In order to do something like this, I've run yarn add gsap because I'm using GreenSock for this animation. In my `index.vue` page, I can remove the existing CSS animation and add this into the `<script>` tags:

```javascript
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
```