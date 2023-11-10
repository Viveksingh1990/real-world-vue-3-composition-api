Vue Router Essentials
=====================

In this lesson we’re going to introduce you to the tools that Vue uses to navigate between pages (or views) in our application. We’ll cover:

*   What is Client-Side Routing?
*   What is a single page application?
*   How is Vue Router setup in a Vue application?
*   Then we’ll customize the routers in our example app

* * *

**Server-Side vs Client-Side Routing**
--------------------------------------

When it comes to websites, typically we connect our page together with links. A link gets clicked, it calls back to the server for the next page, and that page gets loaded.

![https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1.opt.1603835608514.jpg?alt=media&token=3005400a-08eb-4e44-8b55-bfaf0b706e95](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1.opt.1603835608514.jpg?alt=media&token=3005400a-08eb-4e44-8b55-bfaf0b706e95)

We call this “Server-Side Routing” because the client is making a request to the server on every URL change.

![https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F2.opt.1603835634883.jpg?alt=media&token=7dbbd0f9-2443-4942-b044-71ccc79de855](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F2.opt.1603835634883.jpg?alt=media&token=7dbbd0f9-2443-4942-b044-71ccc79de855)

When it comes to Vue, many choose client-side routing, meaning that the routing happens in the browser itself using JavaScript.

![https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F3.opt.1603835634884.jpg?alt=media&token=436ffcea-e024-4ebb-b51d-fefbfffca84e](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F3.opt.1603835634884.jpg?alt=media&token=436ffcea-e024-4ebb-b51d-fefbfffca84e)

In many cases, the view of our app that we need to show has already been loaded into the browser, so we don’t need to reach out to the server for it. Vue Router simply updates what part of the app that is currently being displayed.

![https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F4.opt.gif?alt=media&token=d8f79fe9-dc02-4b1d-8ee5-91fd87e953db](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F4.opt.gif?alt=media&token=d8f79fe9-dc02-4b1d-8ee5-91fd87e953db)

In fact, with routing like this, our app is functioning as a Single Page Application. So what exactly does that mean?

* * *

Single Page Applications
------------------------

A **Single Page Application** (SPA) is a web app that loads from a single page and dynamically updates that page as the user interacts with the app. In our case, everything is being loaded from the **index.html** file of our project. In lesson 2, we looked at that file and saw that it contained this `div` with the id of `#app`.

**📁 index.html**

    <div id="app"></div>
    

We also took a look at **main.js** and learned that when our app is created, it’s being mounted to that `div` with the id of `#app`.

📄 **src/main.js**

    const app = createApp(App)
    
    app.use(createPinia())
    app.use(router)
    
    app.mount('#app')
    

In other words, the **index.html** file is the “single page” of our single page application, where all of the application code is mounted. Vue Router enables client-side routing so we can navigate around and display different “views” of our app.

* * *

**package.json**
----------------

All of our application’s dependencies are tracked inside our **package.json** file. If we take a quick look inside here, we see that the Vue CLI already inserted Vue Router as a dependency because we selected to add it when we configured our project.

📄 **package.json**

    ...
    "dependencies": {
      "axios": "^1.2.1",
      "pinia": "^2.0.21",
      "vue": "^3.2.38",
      "vue-router": "^4.1.5"
    },
    ..
    

This is telling our application to use a version of vue-router that is compatible with version 4.0.0-0 of the vue-router. (Your version number may be different depending on when you take this course)

Earlier in Lesson 2, we ran `npm install`, which went out to NPM, and installed the vue-router library inside our application’s **node\_modules** directory.

Now let’s take a look inside the **router** directory to see how Vue Router is working.

* * *

How Vue Router is configured
----------------------------

Inside the router director, we find the index.js for our router. At the top of this file, we are importing the vue-router library.

**📁 src/router/index.js**

    import { createRouter, createWebHistory } from 'vue-router'
    

And then we import a component we’ll use in our routes:

    import HomeView from '../views/HomeView.vue'
    

And then we use this route:

    routes: [
      {
        path: '/',
        name: 'home',
        component: HomeView
      },
      {
        path: '/about',
        name: 'about',
        ...// Skipping this part, which we will come back later
      }
    ]
    

The `path` indicates the actual route, in terms of the URL, that the user will be taken to. In this first route, there’s only the `/`, meaning this is the root, the homepage of our application, and what people see when they go to our domain at [example.com](http://example.com/).

The `name` allows us to give this route a name so we can use that name throughout our application to refer to this route (more on this later in the course).

The `component` allows us to specify which component to render at that route. Note that `HomeView` was imported at the top of the file. So as it is, the `HomeView` component will be rendered whenever the browser’s URL ends with a `/` with nothing after it.

Taking a look at the second route object, we can see it has a different path:

**📁 src/router/index.js**

    {
      path: '/about',
      name: 'about',
      // route level code-splitting
      // this generates a separate chunk (About.[hash].js) for this route
      // which is lazy-loaded when the route is visited.
      component: () => import('../views/AboutView.vue')
    }
    

When the browser’s URL ends with `/about`, the `About` component will be rendered.

You probably noticed it’s also importing the component differently. Rather than importing it at the top of the file like we did with `HomeView`, we are instead importing it only when the route is actually called. As it says in the comments, this will generate a separate `about.js` file, which will only be loaded into someone’s browser once they navigate to `/about`. This is a performance optimization that isn’t necessary in our simple, small application. But as an application grows, it can be useful to split out how it’s loaded to different JavaScript files, which only get loaded only when they are needed.

* * *

We use `createRouter` to create the router, telling it to use the browser’s History API, and sending in the `routes`, before finally exporting it from this file.

**📁 src/router/index.js**

    const router = createRouter({
      history: createWebHistory(process.env.BASE_URL),
      routes: [
        ...
      ]
    })
    
    export default router
    

So we’ve defined the two different views that our app is going to be able to navigate between, but we actually haven’t yet loaded this router into our Vue instance. Remember, our entire application gets loaded from our **main.js**, and if we look inside this file we can see that we’re importing our **./router/index.js** file, which is bringing in what we exported from **router.js**.

📄 **src/main.js**

    import router from './router' // <-- This imports index.js from the /router directory
    

And in **main.js** you’ll notice that we tell our Vue instance to use the router we’ve imported:

📄 **src/main.js**

    app.use(router)
    

So far so good. Now our router is set up. But where is the functionality added to allow the user to navigate to different parts of the app?

* * *

Built-in Vue Router Components
------------------------------

Looking within **App.vue**, we’ll find a `div` with the id of `#nav`. And inside of it there are some `router-link`s, which are global, Vue Router-specific components we have access to.

📄 **src/App.vue**

    <header>
      <div class="wrapper">
        <nav>
          <RouterLink to="/">Home</RouterLink> |
          <RouterLink to="/about">About</RouterLink>
        </nav>
      </div>
    </header>
    

And below them is this other Vue Router component:

📄 **src/App.vue**

    <RouterView />
    

**So what’s happening here?**

`<RouterLink>` is a component (from the vue-router library) whose job is to link to a specific route. You can think of them like an embellished anchor tag.

`<RouterView/>` is essentially a placeholder where the contents of our “view” component will be rendered onto the page.

When a user clicks on the Home link, where are they taken _to_? That answer lies within the `to` attribute: `<RouterLink to="/">`

They are taken to `/`, which means that according to the route that is set up in **router.js**, the Home component will be loaded.

**📁 src/router/index.js**

    {
      path: '/',
      name: 'home',
      component: HomeView
    },
    

But where, exactly, will it be loaded? The answer is: in the `<RouterView/>`

Again, that is just a placeholder that is replaced by the “view” component we route to, such as **HomeView** or **AboutView**.

![router-link-placeholder.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1.1672252407281.jpg?alt=media&token=0bda6b46-fd8e-4113-8671-8e620a72b752)

* * *

Customizing our **Example App**
===============================

Now that we understand the foundations of Vue Router, we’re ready to start customizing the routes within our example app. Our task list includes:

1.  Rename **HomeView.vue** to **EventListView.vue**
2.  Customize route for **EventListView**
3.  Update **AboutView.vue**
4.  Reconfigure **About** route

* * *

Rename HomeView.vue to EventListView.vue
----------------------------------------

Because **HomeView** is really a list of events, let’s rename it to **EventListView**.

**📁 src/views/EventListView.vue**

    <script setup>
    import EventCard from '@/components/EventCard.vue'
    import { ref } from 'vue'
    
    const events = ref([
    ...
    

* * *

Customize route for EventListView
---------------------------------

Now that the file has been renamed, we’ll need to change our import statement in our router file, and amend the route object itself.

**📁 src/router/index.js**

    import { createRouter, createWebHistory } from 'vue-router'
    import EventListView from '../views/EventListView.vue' // imported renamed SFC
    
    const routes = [
      {
        path: '/',
        name: 'event-list',
        component: EventListView
      },
      ...
    ]
    

* * *

Update AboutView.vue
--------------------

Now let’s add some personalization to the About page to fit our example app, adding this text description.

    <template>
      <div class="about">
        <h1>A site for events to better the world.</h1>
      </div>
    </template>
    

* * *

Reconfigure About route
-----------------------

Since we don’t need to be using route-level code-splitting for our app, we’ll simplify the About route object, like so:

**📁 src/router/index.js**

    {
      path: '/about',
      name: 'about',
      component: AboutView
    }
    

(Don’t forget to import **AboutView.vue**)

At this point, our newly customized router file looks like this:

**📁 src/router/index.js**

    import { createRouter, createWebHistory } from 'vue-router'
    import EventListView from '../views/EventListView.vue'
    import AboutView from '../views/AboutView.vue'
    
    const router = createRouter({
      history: createWebHistory(import.meta.env.BASE_URL),
      routes: [
        {
          path: '/',
          name: 'event-list',
          component: EventListView,
        },
        {
          path: '/about',
          name: 'about',
          component: AboutView,
        },
      ],
    })
    
    export default router
    

* * *

### A final step

Because we’re now displaying that event list on what used to be our “Home” page, let’s update the inner HTML of the `RouterLink` for that view.

📄**App.vue**

    <template>
      <div id="layout">
        <header>
          <div class="wrapper">
            <nav>
              <RouterLink to="/">Events</RouterLink> |
              <RouterLink to="/about">About</RouterLink>
            </nav>
          </div>
        </header>
        <RouterView />
      </div>
    </template>
    

![browser-events.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F2.1672252407282.jpg?alt=media&token=62f0c622-3e4f-4bd7-896d-c1a9c84e6f77)

* * *

**Up Next**
-----------

In our next lesson, we’ll learn how to fetch our events as external data we pull in through an API call using Axios.

![](/images/folder-link-blue.svg)

### Lesson Resources

##### Source Code:

*   [Starting Code](https://github.com/Code-Pop/Real-World-Vue-3-New-Syntax/tree/L4-start)
    
*   [Ending Code](https://github.com/Code-Pop/Real-World-Vue-3-New-Syntax/tree/L4-end)
    
*   [History Mode](https://router.vuejs.org/guide/essentials/history-mode.html#example-server-configurations)
