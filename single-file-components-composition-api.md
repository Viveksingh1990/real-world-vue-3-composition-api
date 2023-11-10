Single File Components
======================

Now that we’ve created our project with **create-vue**, we’re ready to start customizing it to build our own app.

* * *

If you’re coding along (which I encourage you to do) you’ll want to checkout the `L3-start` branch of our [project repo](https://github.com/Code-Pop/Real-World-Vue-3-New-Syntax) to grab the starting code (L3 stands for Lesson 3). In that code, I want to bring your attention to this file I’ve added:

📄 .**prettierrc.json**

    {
      "singleQuote": true,
      "semi": false
    }
    

Here, I’ve set up some rules so that Prettier will change any double quotes (`"`) to single ones (`'`) and remove any semicolons (`;`). I’m not advocating for or against semicolons and double quotes. This is a simple example of how you might add some Prettier configuration rules to your project. We could do something similar for ESLint as well. For a more in-depth look at how you can configure ESLint + Prettier as well as get the most out of VS Code, you can check out [this article](https://www.vuemastery.com/blog/vs-code-for-vuejs-developers).

* * *

What are these .vue files?
--------------------------

In order to start building our app, we need to get some foundational understanding of how things are working within the demo app the CLI created for us, including the **views** directory, which includes two single-file .vue files: **HomeView.vue** and **AboutView.vue**

These are the components that Vue Router loads up when we navigate to the Home and About routes, respectively.

![Screen Shot 2022-12-27 at 9.12.35 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1.1672249981459.jpg?alt=media&token=e026fc59-500c-4be5-8446-37cc2d107fb9)

In the next lesson, we’ll explore the essentials of Vue Router, but for now you just need to understand that these “view” components are the different views that can be seen (or navigated to) within our app. They can contain child components that are nested within them, and their children will be displayed in that view as well. For example, the **HomeView.vue** component has a child: **TheWelcome.vue**, which has a bunch of template code that is being displayed when we’re on the Home route.

Each of these .vue files are single file components, and that’s what this lesson is exploring: How are single file components composed, and how do you use them to create a Vue app?

* * *

Anatomy of a Single File Component
----------------------------------

When we’re talking about Vue apps, we’re really talking about a collection of Vue components.

![https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F2.opt.gif?alt=media&token=35fb74f3-a2b3-4fa6-9b8c-2eb5f2996bf2](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F2.opt.gif?alt=media&token=35fb74f3-a2b3-4fa6-9b8c-2eb5f2996bf2)

So what do these single file components look like under the hood?

![Screen Shot 2022-12-27 at 9.14.19 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F2.1672249981460.jpg?alt=media&token=5472eb73-f9ed-400e-bf18-b5e079535237)

A typical .vue component has three sections: `<script>`, `<template>`, and `<style>`.

To use the analogy of a human body, you can think of the template as the skeleton of your component since it gives it structure, and the script section is the brains, providing the intelligence and behavior. The style section is exactly what it sounds like: the clothing, makeup, hairstyle, etc.

Traditionally, these sections are written in HTML, JavaScript and CSS. However, with the proper setup, you could also use alternatives such as Pug, TypeScript, and SCSS.

Now that we’re starting to understand single file components, we can start building our own. But first, _what_ are we building in this course, exactly?

* * *

The app we’re building
----------------------

By the end of this course, we will have built an app that display events.

![Screen Shot 2022-12-27 at 9.17.17 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F3.1672249986271.jpg?alt=media&token=d8bcdb5f-74f6-4d2e-af3a-0eeb32b7a640)

The events will be pulled in from an external API call, and displayed on the Home page. We’ll be able to click on the event to see the event details.

![Screen Shot 2022-12-27 at 9.19.21 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F4.1672249988554.jpg?alt=media&token=204564a8-f8a8-4afc-a205-4e79888af6f2)

* * *

Our first Single File Component
-------------------------------

To get started building our first component, we’ll simply delete out the code that is within the `<script>`, `<template>`, and `<style>` sections of **HelloWorld.vue**. While we’re at it, let’s rename this file to **EventCard.vue**, since it’s the card that displays info for each event.

![Screen Shot 2022-12-27 at 9.20.58 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F5.1672249991088.jpg?alt=media&token=9a826d9b-6f03-4c7b-9529-64ecdc969c22)

**📁 src/components/EventCard.vue**

    <script setup>
    // defineProps({
    //   msg: {
    //     type: String,
    //     required: true,
    //   },
    // })
    </script>
    
    <template>
      <div class="greetings"></div>
    </template>
    
    <style scoped></style>
    

And since there’s no longer a **HelloWorld** component, we need to remove it from **App.vue:**

**📁 src/App.vue**

    <script setup>
    import { RouterLink, RouterView } from 'vue-router'
    // import HelloWorld from './components/HelloWorld.vue'
    </script>
    
    <template>
      <header>
        <img
          alt="Vue logo"
          class="logo"
          src="@/assets/logo.svg"
          width="125"
          height="125"
        />
    
        <div class="wrapper">
          <!-- <HelloWorld msg="You did it!" /> -->
    
          <nav>
            <RouterLink to="/">Home</RouterLink>
            <RouterLink to="/about">About</RouterLink>
          </nav>
    

Set up the layout
-----------------

While we’re modifying **App.vue**, let’s also change the code here for a new layout.

First, remove the `<img>` tag. Wrap everything in a new `div` (with `id="layout"`). Optionally, add a separator (`|`) between the Home link and the About link.

The `<template>` should look like this now:

**📁 src/App.vue**

    <template>
      <div id="layout">
        <header>
          <div class="wrapper">
            <nav>
              <RouterLink to="/">Home</RouterLink> |
              <RouterLink to="/about">About</RouterLink>
            </nav>
          </div>
        </header>
        <RouterView />
      </div>
    </template>
    

Next, scroll down to `<style>`, and replace the existing CSS with the following:

**📁 src/App.vue**

    #layout {
      font-family: Avenir, Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      color: #2c3e50;
    }
    nav {
      padding: 30px;
    }
    nav a {
      font-weight: bold;
      color: #2c3e50;
    }
    nav a.router-link-exact-active {
      color: #42b983;
    }
    

With this new CSS code, we’re not going to need the default **main.css** imported in **main.js**.

So, let’s remove the `main.css` import from **main.js**:

**📁 src/main.js**

    import { createApp } from 'vue'
    import { createPinia } from 'pinia'
    
    import App from './App.vue'
    import router from './router'
    
    // import './assets/main.css'
    

* * *

While we’re at it, let’s also remove some component files we no longer need.

*   all the files in **src/components/icons**
*   **TheWelcome.vue**
*   **WelcomeItem.vue**

* * *

Finally, we need to go to the **HomeView.vue** and remove **TheWelcome.vue** import. And replace the template code with a new `div` (with `class="home"`).

**HomeView.vue** should look like this now:

    <script setup></script>
    
    <template>
      <div class="home"></div>
    </template>
    

With all of the above changes, you should now be able to see the new layout in the browser:

![Screen Shot 2022-12-27 at 10.14.25 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F6.1672249993387.jpg?alt=media&token=b6deb041-0527-4cb7-8614-b84de12d595a)

And the page content will appear right in the center:

![Screen Shot 2022-12-27 at 10.16.15 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F7.1672249997671.jpg?alt=media&token=858b3a77-9547-4e0a-80d9-8cf5691f5a3d)

Now that we have our layout ready, let’s continue building the **EventCard** component.

* * *

Back to EventCard
-----------------

First, let’s add some styles. We’ll change the class name on the `div` in order to do that.

**📁 src/components/EventCard.vue**

    <script setup>
    // defineProps({
    //   msg: {
    //     type: String,
    //     required: true,
    //   },
    // })
    </script>
    
    <template>
      <div class="event-card"></div>
    </template>
    
    <style scoped></style>
    

And add some styles for the event-card (including a hover effect):

**📁 src/components/EventCard.vue**

    .event-card {
      padding: 20px;
      width: 250px;
      cursor: pointer;
      border: 1px solid #39495c;
      margin-bottom: 18px;
    }
    .event-card:hover {
      transform: scale(1.01);
      box-shadow: 0 3px 12px 0 rgba(0, 0, 0, 0.2);
    }
    

Now the `div` has the proper styles, including a hover effect. If you’re wondering what that `scoped` attribute means, that allows us to _scope_ and isolate these styles to just this component. This way, these styles are specific to this component and won’t affect any other part of our application. You’ll see me using `scoped` styles throughout this course.

Since we want to display information about the event on this **EventCard**, we need to give it an event to display. So let’s add that using a `ref` in our `<script>` section.

**📁 src/components/EventCard.vue**

    <script setup>
    import { ref } from 'vue'
    
    // defineProps({
    //   msg: {
    //     type: String,
    //     required: true,
    //   },
    // })
    
    const event = ref({
      id: 5928101,
      category: 'animal welfare',
      title: 'Cat Adoption Day',
      description: 'Find your new feline friend at this event.',
      location: 'Meow Town',
      date: 'January 28, 2022',
      time: '12:00',
      petsAllowed: true,
      organizer: 'Kat Laydee',
    })
    </script>
    

Now, in the `<template>` we can display some of that `event` data with JavaScript expressions, like so:

**📁 src/components/EventCard.vue**

    <template>
      <div class="event-card">
        <h2>{{ event.title }}</h2>
        <span>@{{ event.time }} on {{ event.date }}</span>
      </div>
    </template>
    

That’s it for the component for now.

* * *

In order for this **EventCard** to be displayed, it needs to be put somewhere that can be routed to, such as the **HomeView.vue** file in our views directory. We’ll need to import **EventCard.vue**, and then we can use it in the template.

**📁 src/views/HomeView.vue**

    <script setup>
    import EventCard from '@/components/EventCard.vue'
    </script>
    
    <template>
      <div class="home">
        <EventCard />
      </div>
    </template>
    

Now, we should be seeing our EventCard showing up in the browser when we’re on the Home view.

![Screen Shot 2022-12-27 at 10.31.34 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F8.1672249997672.jpg?alt=media&token=10113933-5dff-4997-8a5e-311a979dba51)

* * *

Refactoring for a more production-ready use case
------------------------------------------------

We’re making great progress, but remember we want the **EventCard** to be displaying in the middle of the Home page. And, since we’ll eventually have a collection of events that we pull in from an API call, we need to do a bit of refactoring to make this a more production-ready use case.

Our refactoring steps include:

*   Move events data to parent (**HomeView.vue**)
*   Parent creates **EventCard** component for each event in its data
*   Parent feeds each **EventCard** its own event to display
*   Parent displays **EventCard**s in a Flexbox container

Let’s get started with this refactor.

* * *

Move events data to parent
--------------------------

Our first step is to delete out the `event` data from **EventCard.** We’ll then add an `event` prop instead, so that the parent can feed this component an event object to display. We’re then left with this code:

**📁 src/components/EventCard.vue**

    <script setup>
    import { ref } from 'vue'
    
    defineProps({
      event: {
        type: Object,
        required: true,
      },
    })
    </script>
    
    <template>
      <div class="event-card">
        <h2>{{ event.title }}</h2>
        <span>@{{ event.time }} on {{ event.date }}</span>
      </div>
    </template>
    
    <style scoped>
    .event-card {
      padding: 20px;
      width: 250px;
      cursor: pointer;
      border: 1px solid #39495c;
      margin-bottom: 18px;
    }
    .event-card:hover {
      transform: scale(1.01);
      box-shadow: 0 3px 12px 0 rgba(0, 0, 0, 0.2);
    }
    </style>
    

Now that EventCard is set up to receive an event, we can add the `events` data to the parent, **HomeView.vue**.

**📁 src/views/HomeView.vue**

    <script setup>
    import EventCard from '@/components/EventCard.vue'
    import { ref } from 'vue'
    
    const events = ref([
      {
        id: 5928101,
        category: 'animal welfare',
        title: 'Cat Adoption Day',
        description: 'Find your new feline friend at this event.',
        location: 'Meow Town',
        date: 'January 28, 2022',
        time: '12:00',
        petsAllowed: true,
        organizer: 'Kat Laydee',
      },
      {
        id: 4582797,
        category: 'food',
        title: 'Community Gardening',
        description: 'Join us as we tend to the community edible plants.',
        location: 'Flora City',
        date: 'March 14, 2022',
        time: '10:00',
        petsAllowed: true,
        organizer: 'Fern Pollin',
      },
      {
        id: 8419988,
        category: 'sustainability',
        title: 'Beach Cleanup',
        description: 'Help pick up trash along the shore.',
        location: 'Playa Del Carmen',
        date: 'July 22, 2022',
        time: '11:00',
        petsAllowed: false,
        organizer: 'Carey Wales',
      },
    ])
    </script>
    
    <template>
      <div class="home">
        <EventCard />
      </div>
    </template>
    

* * *

Parent creates **EventCard** components
---------------------------------------

Now that **HomeView.vue** has the `events` data, we can use that data to create a new **EventCard** for each of the event objects that are in that data, using the `v-for` directive.

**📁 src/views/HomeView.vue**

    <template>
      <div class="home">
        <EventCard v-for="event in events" :key="event.id" :event="event" />
      </div>
    </template>
    

Notice how we’re binding the event’s `id` to the `:key` attribute. This gives Vue.js a way to identify and can keep track of each unique **EventCard**.

* * *

Parent feeds each **EventCard** its own event
---------------------------------------------

Additionally, as we iterate over the `events` array to create a new **EventCard** for each event object, we’re passing in that `event` object into a new `:event` prop we’ve added to the **EventCard**. This way, each **EventCard** has all of the data it needs to display its own event info.

* * *

Parent displays **EventCard**s in a Flexbox container
-----------------------------------------------------

If we check this out in the browser, it’s working. We’ve created an EventCard for each of the `events` in **HomeView.vue**’s data.

![Screen Shot 2022-12-27 at 10.41.55 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F9.1672250000870.jpg?alt=media&token=45e7d4d6-407e-4a2c-87dc-2b2ed5cd00a4)

Finally, we just need to put these events in a Flexbox container to get things looking how we want. Let’s head into the **HomeView.vue** file and change the class name of the `div` that our `EventCard` is nested within, and add some Flexbox styles.

**📁 src/views/HomeView.vue**

    ...
    
    <template>
      <div class="events">
        <EventCard v-for="event in events" :key="event.id" :event="event" />
      </div>
    </template>
    
    <style scoped>
    .events {
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    </style>
    

Now, our **EventCard**s will be displayed within a center-aligned column.

* * *

What about Global Styles?
-------------------------

So far, we’ve discussed scoped styles and how the `scoped` attribute allows us to add styles that target the specific component we’re concerned about. But what about global styles that we want applied to our entire app? While there are different ways to achieve this, the simplest way to get started with this is by heading into the **App.vue** file. Remember: this is the root component of our app.

And remove `scoped` from `<style>`:

**📁 src/App.vue**

    <style>
    ...
    </style>
    

Now all of styles in **App.vue** are applied to the entire app. Here, we could add a new rule. Like so:

**📁 src/App.vue**

    <style>
    ...
    h2 {
      font-size: 20px;
    }
    </style>
    

Now, any `h2` in our app will have a font-size of `20px`. Since our **EventCard**’s template has an `h2`, that element will receive this new global style.

**📁 src/components/EventCard.vue**

    <template>
      <div class="event-card">
        <h2>{{ event.title }}</h2>
        <span>@{{ event.time }} on {{ event.date }}</span>
      </div>
    </template>
    

Speaking of global items in our Vue app, what would happen if we added something like an `h1` to our **App.vue**’s template?

**📁 src/App.vue**

    
    <template>
      <div id="layout">
        <header>
          <div class="wrapper">
            <nav>
              <RouterLink to="/">Home</RouterLink> |
              <RouterLink to="/about">About</RouterLink>
            </nav>
          </div>
        </header>
        <h1>Events For Good</h1> <!-- new element -->
        <RouterView />
      </div>
    </template>
    

Let’s head into the browser and take a look.

![Screen Shot 2022-12-27 at 10.50.00 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F10.1672250003121.jpg?alt=media&token=5f69d3a1-a026-4e72-93d0-6c51bb054ae8)

We’re seeing a few things. First, our Flexbox container is working (✅) and the event titles are now just a bit bigger (`20px`) due to that new global `h4` style rule we added. And notice what happens when we navigate to the About route.

![Screen Shot 2022-12-27 at 10.50.52 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F11.1672250005403.jpg?alt=media&token=8d312bf8-b0fb-48fe-9a2c-0cd03dae5090)

(Notice that the content of this page _"This is an about page”_ shows up in the corner, we’ll fix this positioning in a bit.)

We’re still seeing that `h1` displaying “Events For Good”. So this tells us that we can place content in our **App.vue**’s template that we want to be displayed globally across every view of our application. This could be useful for things a search bar, header, or of course a nav bar like we already have here.

But for our use case, we don’t need that title showing up in every view, so we’ll place it into the **HomeView.vue** file.

**📁 src/views/HomeView.vue**

    ...
    <template>
      <h1>Events For Good</h1>
      <div class="events">
        <EventCard v-for="event in events" :key="event.id" :event="event" />
      </div>
    </template>
    ...
    

Now that title will only show up one the Home route.

* * *

Finally, to fix the positioning issue for the About page, we just need to remove the following default styles from **AboutView.vue**:

    <template>
      <div class="about">
        <h1>This is an about page</h1>
      </div>
    </template>
    
    <style>
    /* @media (min-width: 1024px) {
      .about {
        min-height: 100vh;
        display: flex;
        align-items: center;
      }
    } */
    </style>
    

Now it should look like this:

![Screen Shot 2022-12-27 at 10.58.42 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F12.1672250009636.jpg?alt=media&token=15d677eb-264e-4172-a562-9fb8206761b8)

* * *

Let’s ReVue
-----------

We’ve covered a lot. We learned what a single file .vue component is, how it’s composed (with `scoped` versus global styles), and how to start using these .vue components to build up a Vue app. In the next lesson, we’re going to dive deeper into the essentials of Vue Router to better understand how to set up app navigation. See you there!
