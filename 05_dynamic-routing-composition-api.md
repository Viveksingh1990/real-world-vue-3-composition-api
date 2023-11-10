Dynamic Routing
===============

In this lesson, we’re going to add the functionality where a user can click any of the **EventCard**s that are displayed on our homepage and be routed to a view that shows more details about that event. In other words: we’re going to implement some dynamic routing behavior. We’ll tackle this new feature in two parts.

* * *

Part 1: What we’ll achieve
--------------------------

*   Create a new **EventDetailsView** component to display the event’s details
*   Add a new API call to fetch a single event by its id (this is the event we’ll display the details of)
*   Add a route for the new **EventDetailsView** component
*   Make **EventCard** clickable so we can access this new **EventDetailsView** route

* * *

Create EventDetailsView Component
---------------------------------

First up, we’ll create the component to display the event details, adding it to our **views** directory.

**📁 src/views/EventDetailsView.vue**

    <script setup>
    import { ref, onMounted } from 'vue'
    
    const event = ref(null)
    
    onMounted(() => {
      // fetch event (by id) and set local event data
    })
    </script>
    
    <template>
      <div>
        <h1>{{ event.title }}</h1>
        <p>{{ event.time }} on {{ event.date }} @ {{ event.location }}</p>
        <p>{{ event.description }}</p>
      </div>
    </template>
    

It renders out the details from the `event` ref. That event is retrieved from an API call that fetches it, by its id. Let’s revisit our mock database to see how to fetch it.

* * *

Add API call to fetch event by id
---------------------------------

Notice what happens when we call up our my-json-server url, this time with an id at the end of it (_…/events/123_). This targets a single event, where its `id` matches the end of our url: `123`.

![https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1.opt.1607454318295.jpg?alt=media&token=a7fd9f52-2983-4404-ab49-c2451db7197b](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1.opt.1607454318295.jpg?alt=media&token=a7fd9f52-2983-4404-ab49-c2451db7197b)

This is the kind of url we’ll use when fetching a single event, where it ends with the event’s id. Let’s head into our **EventService** file and add that API call now.

**📁 src/services/EventService.js**

    import axios from 'axios'
    
    const apiClient = axios.create({
      baseURL: 'https://my-json-server.typicode.com/Code-Pop/Real-World_Vue-3',
      withCredentials: false,
      headers: {
        Accept: 'application/json',
        'Content-Type': 'application/json'
      }
    })
    
    export default {
      getEvents() {
        return apiClient.get('/events')
      },
      //Added new call
      getEvent(id) {
        return apiClient.get('/events/' + id)
      }
    }
    

The `getEvent` call is very similar to the `getEvents` one from the last lesson. However, it takes in an `id` as its argument, which is appended to the end of the url we’re making a `get` request to.

Now that the `getEvent` call is ready to use, let’s use it within our new **EventDetailsView** component.

**📁 src/views/EventDetailsView.vue**

    import { ref, onMounted } from 'vue'
    import EventService from '@/services/EventService.js'
    
    const event = ref(null)
    const id = ref(123)
    
    onMounted(() => {
      EventService.getEvent(id.value)
        .then((response) => {
          event.value = response.data
        })
        .catch((error) => {
          console.log(error)
        })
    })
    

A few things to note here:

*   We are calling `getEvent` from the **EventService**, which we’ve now imported into the component
*   We’re passing in `id.value` - That `id` is currently just a hard-coded data value. (We’ll make this dynamic in part 2 of this lesson. This is not our ultimate solution.)
*   We’re setting our local `event` ref equal to the response of our `getEvent` request

Now that this component is making a call out for a single event to display, we can add this component to our routes.

* * *

Add EventDetailsView as a route
-------------------------------

We’ll head into our router file, import **EventDetailsView**, and add it to our `routes` array:

**📁 src/router/index.js**

    ...
    import EventDetailsView from '../views/EventDetailsView.vue'
    import AboutView from '../views/AboutView.vue'
    
    const router = createRouter({
      history: createWebHistory(import.meta.env.BASE_URL),
      routes: [
        ...
        {
          path: '/event/123',
          name: 'event-details',
          component: EventDetailsView,
        },
        ...
      ],
    })
    

For now, we’ll just hard-code the path: `'/event/123'`. Eventually, the end part (`123`) will be dynamic, and updated with the id of the event that is currently being displayed.

Now that we have this new route, we need to be able to access it. Again, we’re wanting to access this route whenever we click on one of the **EventCards** on our homepage.

* * *

Make EventCard clickable with a RouterLink
------------------------------------------

Heading into the **EventCard** component, let’s wrap our template code in a `RouterLink`

**📁 src/components/EventCard.vue**

    <template>
      <RouterLink to="event/123">
        <div class="event-card">
          <h2>{{ event.title }}</h2>
          <span>@{{ event.time }} on {{ event.date }}</span>
        </div>
      </RouterLink>
    </template>
    

Now, when one of our EventCards is clicked, we’ll be routed to the new path `event/123`.

* * *

If we check this out in the browser, we’ll see an error in the console if we click on an event:

![event-error.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1.1672339263863.jpg?alt=media&token=658a725c-7ea8-4ce8-b1f0-959d5d0f3031)

What’s happening here is that **EventDetailsView** is trying to display the event’s details before it has received the event back from the API call. We need to tell our component to wait until it has the event before trying to display its details. Fortunately, that is a very simple fix.

Fortunately, this is a very quick fix.

In **EventsDetailsView.vue**, add a `v-if` on the div:

**📁 src/views/EventDetailsView.vue**

    <template>
      <div v-if="event">
        <h1>{{ event.title }}</h1>
        <p>{{ event.time }} on {{ event.date }} @ {{ event.location }}</p>
        <p>{{ event.description }}</p>
      </div>
    </template>
    

By adding a simple `v-if="event"` on our div here, we can make sure it only renders when the `event` exists in our data.

If we check again in the browser, we’ll see that it’s working so far… When we click on the Cat Adoption Day **EventCard**, we’re taken to a view that display’s the details of that event.

![cat-event.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F2.1672339263864.jpg?alt=media&token=63414578-3e1a-4c8c-a74e-0000e751b449)

However, if we click on any other **EventCard**, we’re still pulling up the same Cat Adoption Day details, and the id at the end of our url is the same: 123. That’s expected, since we hardcoded the id we are passing into the `getEvent` call, and in the path of the EventDetails route.

This brings us to the end of Part 1 and to the beginning of Part 2, where we make this routing behavior dynamic so we can route to the details of any **EventCard** we click on.

* * *

Part 2: Making it Dynamic
-------------------------

To make our routing behavior dynamic, we need to switch out the hard-coded id in our path (`/123`) and replace it with a **dynamic segment**. This is basically a variable _parameter_ for the url path, which gets updated with the id of whichever event is currently displayed on that route.

![id-as-prop.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F3.1672339268875.jpg?alt=media&token=21ef849a-ff4a-4e9e-8c4d-f06b168b2898)

We’ll then want to be able to feed that dynamic segment into the **EventDetailsView** component as a prop to be used when making the `getEvent` call.

* * *

Add a dynamic segment to EventDetailsView route
-----------------------------------------------

Let’s get started, and add a dynamic segment to the path of the **EventDetailsView** route.

**📁 src/router/index.js**

    {
      path: '/event/:id',
      name: 'event-details',
      props: true,
      component: EventDetailsView,
    },
    

Notice how the syntax for a dynamic segment begins with a colon `:` and is followed by whatever you want to call the segment. In this case, it’s `:id` since it gets replaced with our event’s id. In another use case, this could be something like `:username` or `:orderNumber`.

We’ve also added `props: true` here to give the **EventDetailsView** component access to this dynamic segment parameter as a prop.

Since we’ve updated the path in this route, the path in the `to` attribute of our **EventCard**’s attribute now needs to be updated. Remember, it’s currently hardcoded as `to="event/123"`

📁 **src/components/EventCard.vue**

    <template>
      <RouterLink to="event/123">
        <div class="event-card">
          <h2>{{ event.title }}</h2>
          <span>@{{ event.time }} on {{ event.date }}</span>
        </div>
      </RouterLink>
    </template>
    

A cleaner solution here would be to simply use a **named route**, where we bind `:to` an object that specifies which route this link routes to.

📁 **src/components/EventCard.vue**

    <RouterLink :to="{ name: 'event-details' }">
    

* * *

💡**Relevant Tangent**: Now, we’ve also made our app a bit more scalable. In a bigger app with `RouterLink`s throughout it, it becomes unnecessarily strenuous to maintain the paths in each `RouterLink` whenever they need to change. On the other hand, if your `RouterLink`s used **named routes**, and your route’s path needs to change, you can simply change it once in the router file, and none of your `RouterLink`s need to be updated since they aren’t relying on the path itself.

* * *

Add event id to router’s parameters
-----------------------------------

At this point you might be wondering how we tell our dynamic `:id` segment what value it needs to be replaced by. We can do so by adding the `params` property onto our object here in the `to` attribute:

📁 **src/components/EventCard.vue**

    <script setup>
    defineProps({
      event: {
        type: Object,
        required: true,
      },
    })
    </script>
    
    <template>
      <RouterLink :to="{ name: 'event-details', params: { id: event.id } }">
        <div class="event-card">
          <h2>{{ event.title }}</h2>
          <span>@{{ event.time }} on {{ event.date }}</span>
        </div>
      </RouterLink>
    </template>
    

Remember from a few lessons ago, this component has the `event` as a prop, so we can grab `event.id` from it and set the params `id` equal to it.

    <RouterLink :to="{ name: 'event-details', params: { id: event.id } }">
    

Now, when we click on this `RouterLink`, we’re routed to **EventDetailsView** and the route’s path is appended with the event’s id.

We can now finally feed that `id` param into the **EventDetailsView** component as a prop.

📁 **src/views/EventDetailsView.vue**

    <script setup>
    import { ref, onMounted } from 'vue'
    import EventService from '@/services/EventService.js'
    
    const props = defineProps({
      id: {
        required: true,
      },
    })
    
    const event = ref(null)
    
    onMounted(() => {
      EventService.getEvent(props.id)
        .then((response) => {
          event.value = response.data
        })
        .catch((error) => {
          console.log(error)
        })
    })
    </script>
    

Now, when we say `getEvent(this.id)`, we’re referring to the newly added `id` prop. When **EventDetailsView** is routed to and thus _mounted_, it now makes a request for the event with the id that is found in the dynamic parameter of the route’s path.

* * *

If we check this out in the browser, we’re successfully able to click on an **EventCard** and display the proper details for that event.

![Screen Shot 2022-12-28 at 4.31.12 PM.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F4.1672339274007.jpg?alt=media&token=2f277a5f-e4e4-4572-a590-ac3acff3d0aa)

* * *

Cleaning up our Code
--------------------

With that, we’ve finished our dynamic routing behavior. Phew! That was a lot of steps. Now, I just want to clean a few things up before we end.

First, our **EventCards** don’t look as nice now that they’re wrapped with a `RouterLink`

![event-link.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F5.1672339277557.jpg?alt=media&token=d89b0170-d1f7-4262-bef7-c5d92d7c48b8)

Let’s add an `event-link` class to the `<RouterLink>` to make it look nicer:

**📁  src/components/EventCard.vue**

    <template>
      <RouterLink
        class="event-link"
        :to="{ name: 'event-details', params: { id: event.id } }"
      >
        <div class="event-card">
          <h2>{{ event.title }}</h2>
          <span>@{{ event.time }} on {{ event.date }}</span>
        </div>
      </RouterLink>
    </template>
    
    <style scoped>
    ...
    .event-link {
      color: #2c3e50;
      text-decoration: none;
    }
    </style>
    

![event-link-fixed.png](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F6.1672339277558.jpg?alt=media&token=d4b67294-08a1-43bd-833c-06c6397d9e6c)

* * *

For consistency’s sake, we can also update the **App.vue** file to use named routes instead of hardcoded paths.

**📁 src/App.vue**

    <div class="wrapper">
      <nav>
        <RouterLink :to="{ name: 'event-list' }">Events</RouterLink> |
        <RouterLink :to="{ name: 'about' }">About</RouterLink>
      </nav>
    </div>
    

Again, this helps build in scalability to the maintenance of our app’s routes.

* * *

Next steps
----------

To continue learning about concepts like route params and other Vue Router topics, you can check out our entire [Touring Vue Router](https://www.vuemastery.com/courses/touring-vue-router/receiving-url-parameters) course.

In the next lesson, we’re going to learn how to take our app and deploy it into production, using [Render](https://render.com/).

### Lesson Resources

##### Source Code:

*   [Starting Code](https://github.com/Code-Pop/Real-World-Vue-3-New-Syntax/tree/L6-start)
    
*   [Ending Code](https://github.com/Code-Pop/Real-World-Vue-3-New-Syntax/tree/L6-end)
