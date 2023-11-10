Scaling the app
===============

We’ve reached the final lesson of this course, and I want to congratulate you for following along until the end. We’ve built a simple yet solid Vue app and deployed it out into the world. So where do we go from here? There are a number of features we could add, and additional concepts within the Vue framework (and ecosystem of tools) to learn. So what should a Vue developer master next?

In this lesson, I’m going to give a tour of what potential next steps we could take to scale up our app in different directions. This will simultaneously provide some guidance around how to continue with your Vue learning journey and how to best utilize the Vue Mastery platform to level up your skills.

* * *

What’s next?
------------

If you take a look at our courses page, you’ve probably already noticed we have our library of content arranged in different paths. These paths are arranged in the _suggested_ order for consuming our courses. Having said that, your ideal next course ultimately depends on your current needs and pressing interests.

So let’s explore some of the main Vue concepts and how a number of our course addresses them.

* * *

Vue 3 Forms
-----------

If we continued building out our Events for Good app beyond this course, we’d need a production-ready form that enables users to create new events. Forms are arguably one of the most important parts of any app, allowing us to intake important information from our users. A well done form (or a poorly done form) can ultimately affect a company’s bottom line in significant ways.

When building forms with Vue, you don’t just want to build a form that serves a specific purpose. You’ll want to know how to build a set of base _form_ _components_ that are highly reusable so they can be used and reused in a modular way throughout any of your Vue apps.

From inputs to checkboxes and radio buttons, our [Vue 3 Forms](https://www.vuemastery.com/courses/vue3-forms/forms-introduction) course walks through best practices as you develop a set of form components you can take with you into any of your future projects.

* * *

Pinia for State Management
--------------------------

If we’ve implemented the ability for users to create new events, we’ll want a nice global location to store those events once they’re created. As our app’s component tree grows, it becomes increasing harder to manage the data within our app; as it’s created, updated, and displayed. This brings us to Pinia: Vue’s official library for state management.

When we created our project with the create-vue CLI, we selected Pinia. But what purpose, exactly, does it serve?

![pinia-stores.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1.1672340685348.jpg?alt=media&token=ff3c177d-5d93-4bc1-8649-66046e7aaf16)

As a Vue app grows, and you have more and more views and components within those views, you’ll need to have a reliable and predictable way to manage how that data is stored, sent around, and changed, within your app.

Notice how this **store** folder was installed into our project by the CLI. You can think of Pinia as the **storage** center for data throughout your app. Let’s look inside the sample store file:

📁 **stores/counter.js**

    import { ref, computed } from 'vue'
    import { defineStore } from 'pinia'
    
    export const useCounterStore = defineStore('counter', () => {
      const count = ref(0)
      const doubleCount = computed(() => count.value * 2)
      function increment() {
        count.value++
      }
    
      return { count, doubleCount, increment }
    })
    

In short, a **store** provides us with what we need to keep track of the `state` of things within our app, and update that state in an organized _modular_ way that is predictable and traceable.

Our [Pinia Fundamentals](https://www.vuemastery.com/courses/pinia-fundamentals/) course walks you through how to effectively use this state management library so things don’t get messy as you scale up your app’s data needs.

* * *

Touring Vue Router
------------------

While we covered the essentials of Vue Router in this course, I’ve mentioned how there are plenty more navigation-based abilities available to us with this handy routing library, and our [Touring Vue Router](https://www.vuemastery.com/courses/touring-vue-router/vue-router-introduction) course covers many of them.

![https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F3.1609256675823.jpg?alt=media&token=d1399229-46af-44fb-b2ef-e4666ebb8489](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F3.1609256675823.jpg?alt=media&token=d1399229-46af-44fb-b2ef-e4666ebb8489)

So if your app needs features like pagination, guards on certain routes that limit user access, or if you need to enable programmatic navigation or nested routes, this course takes you beyond the essentials and tours various features within this routing toolset.

* * *

Beyond the beginner’s path
--------------------------

If you take the courses outlined so far, you’ll have broadened and reinforced your knowledge of fundamental production-level Vue practices. You’ll then be ready for our more intermediate courses that cover everything from strengthening your Vue apps with Unit Testing, to adding User Authentication, and integrating with popular libraries within the Vue.js ecosystem such as Nuxt.js and Vuetify.

* * *

You’ve made it
--------------

With that, I hope you feel clear about what your next steps are on your path to Vue Mastery. I wish you well as you level up your skills and expand the opportunities that become available to you as you become increasingly skilled with this powerful JavaScript framework. See you in another course!
