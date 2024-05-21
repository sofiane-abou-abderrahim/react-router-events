# Single-Page Application Routing

## Multiple Pages in Single-Page Apps

- What Client Side Routing is & Why Use it?
- Using React Router
- Data Fetching & Submission

# Steps

## 0. Starting Project

1. create a new `README.md` file
2. run `cd backend && npm install && npm start`
3. run `cd frontend && npm install && npm start`
4. add a new `.gitignore` file & put `node_modules` inside

## 1. Time to Practice: Solution

1. Add five new (dummy) page components (content can be simple `<h1>` elements)

   - HomePage
   - EventsPage
   - EventDetailPage
   - NewEventPage
   - EditEventPage

   1. run `cd frontend/ && npm install react-router-dom`
   2. add a new `pages` folder & add inside of it the 5 dummy pages

2. Add routing & route definitions for these five pages

   - / => HomePage
   - /events => EventsPage
   - /events/`<some-id>` => EventDetailPage
   - /events/new => NewEventPage
   - /events/`<some-id>`/edit => EditEventPage

   1. in `App.js`, use `RouterProvider` for the route definitions
   2. then, use `createBrowserRouter()` for rendering the routes

3. Add a root layout that adds the `<MainNavigation>` component above all page components

   1. add a new `Root.js` file in the `pages` folder
   2. inside of it, render the `<MainNavigation>` & `<Outlet>` components
   3. in `App.js`, define the `<RootLayout>`
   4. convert the paths into relative paths to the parent root
   5. turn the `HomePage` path into an index route

4. Add properly working links to the MainNavigation

   1. in `MainNavigation.js`, replace `<a>` by `<Link>`
   2. set the `to` attribute to absolute paths

5. Ensure that the links in MainNavigation receive an "active" class when active

   1. in `MainNavigation.js`, use the special `<NavLink>` component instead of the regular `<Link>` component
   2. add the `className` prop & set to it a function with the `isActive` object as a parameter
   3. set the `end` prop to the Home Navlink

6. Output a list of dummy events to the EventsPage

   - Every list item should include a link to the respective EventDetailPage

   1. in `Events.js`, add some `DUMMY_EVENTS` array
   2. render the list of events dynamically

7. Output the ID of the selected event on the EventDetailPage

   1. in `EventDetail.js`, import `useParams` from `react-router-dom` & store the returned value in a `params` constant
   2. use the `params` constant to output the event id

8. BONUS: Add another (nested) layout route that adds the `<EventNavigation>` component above all /events... page components

   1. in `Add.js`, add a new route with a `path` set to `events`
   2. add a new `EventsRoot.js` file inside of the `pages` folder

## 2. Data Fetching with a loader()

1. replace the content of the `Events.js` file with the provided one
2. in `App.js`, add the extra `loader` property to your route definitions so that the fetched data load before the component

## 3. Using Data From A Loader In The Route Component

1. in `Events.js`, get access to the data returned by the `loader` function with help of `useLoaderData`
2. store the returned value of this `userLoaderData` function in a `events` constant
3. pass this `events` constant as a value to the `events` prop of the `<EventList>` component

## 4. More loader() Data Usage

1. use the `useLoaderData` hook in `EventsList.js` instead of `Events.js`
2. don't use `useLoaderData` in higher level from where you defined the `loader` function
3. put back the `useLoaderData` usage in `Events.js`

## 5. Where Should loader() Code Be Stored?

1. put the `loader` function code into your component file where you need it, so in `Events.js`
2. in `Events.js`, export a function that you could name `loader()` & put your `loader` code into that function
3. in `App.js`, import that `{loader}` function & give it an alias like `eventsLoader`
4. use that `eventsLoader` pointer a that function as a value for that `loader` property

## 6. When Are loader() Functions Executed?

1. in `backend\routes\events.js`, add a timer inside the code responsible to send the events data to the frontend
2. quit the server & restart it
3. by default, React Router will wait for the data to be fetched (so for the loader to be finished) before it then renders the page with the fetched data
4. therefore, you don't need to render a loading state on the `EventsPage` component

## 7. Reflecting The Current Navigation State in the UI

1. give the user some feedback that something is going when he clicks on the `Events` navigation link
   1. in `Root.js`, use the `useNavigation` hook
   2. use the `state` property of this `navigation` object returned from the `useNavigation` hook to show a loading text dynamically
2. remove this loading text & the `useNavigation`
3. remove the timer in `backend\routes\events.js`

## 8. Returning Responses in loader()s

1. in `Events.js`, create a new response object by instantiating the built-in `Response` constructor function in the browser
2. whenever you return such a response in your loaders, the React Router package will automatically extract the data from your response when using `useLoaderData`
3. so you can take that `response`object you get from `fetch()` and return that in your `loader()`
4. you don't need to manually extract the data from the response `resData`, instead you can return your `response` straight away
5. and `useLoaderData` will then automatically give us the data that os part of the `response`
6. now, store the returned value of `useLoaderData` in a `data` constant & extract your `events` from that `data` object

## 9. Error Handling with Custom Errors

1. in `Events.js`, if the response is not sent, return an object with `isError` & `message` keys
2. in the `EventsPage` component, check if the `isError` is truthy & render `data.message` in paragraph
3. as an alternative, you can `throw` an error & construct a new error object with the built-in `new Error` constructor or any kind of object
4. add an `Error.js` page
5. in `App.js`, add the `errorElement` property to the root route & render the `<ErrorPage>` component

## 10. Extracting Error Data & Throwing Responses

1. add a new `PageContent.js` file inside of the `components` folder
2. render the `<PageContent>` component in `Error.js`
3. differenciate between the different types of errors by throwing a `new Response` instead of an object in `Events.js` & set the `status` to `500`
4. get hold of the data that is being thrown as an error inside of the component that is being rendered as an `errorElement` with help of the special `{useRouteError}` hook imported from`react-router-dom`
5. use the `status` field of this `{useRouteError}` hook that is provided when you throw a `new Response` which in our case reflects the `500` status of the response we threw

## 11. The json() Utility Function

1. in `Events.js`, instead of throwing a `new Response`, `throw` the `json()` helper utility imported from `react-router-dom`
2. this function creates a response object that includes data in the JSON format
3. now you can remove the `JSON.parse()` function in the place where you use that response data (in our case in `Error.js`)

## 12. Dynamic Routes & loader()s

1. in `EventsList.js`, use the `<Link>` component to navigate to the `EventDetailPage` component upon clicking on an event
2. in `EventDetail.js`, output the `<EventItem>` component
3. set to it the `event` prop & pass to it the event data for the event for which you want to view the details
   1. load the event details by exporting another `loader()` function
   2. use the `params` object to get the `eventId`
   3. register the `loader` in the route definitions to the `<EventDetailPage>` route & import the loader from `EventDetail.js` in `App.js`
   4. in `EventDetail.js`, use the `useLoaderData` hook to get the event data

## 13. The useRouteLoaderData() Hook & Accessing Data From Other Routes

1. in `EventItem.js`, use the `<Link>` component to redirect the user to the `EditEventPage` upon a click
2. in `EditEvent.js`, show the `<EventForm>` component
3. prepopulate that event form with the data for the event which you try to add it
   1. as we fetched the `EventDetail` data on the `EventDetailPage`, fetch it also on the `EditEventPage` because that where we need the data for the `EventForm`
   2. in `App.js`, add a new route definition & set the `path` to `eventId` & just set `children`
   3. add a `loader` to this new route & set the `eventDetailLoader` to it
   4. in `EditEvent.js`, use the `useLoaderData` hook
   5. in `EventForm.js`, use the event data to set it as a default value to this form with the `defaulltValue` prop provided by React
   6. to make sure that you use the `loader` data from the parent route, you must add the special `id` property in `App.js`
   7. in `EventDetail.js` & `EditEvent.js`, use the `useRouteLoaderData` hook instead of `useLoaderData` & pass to it the `event-detail` id

## 14. Planning Data Submission

1. display the `<EventForm>` component in `NewEvent.js`
2. plan how you will send data to the backend API when clicking on the `Save` button
   1. function `submitHandler` (regular method)
   2. or add `actions` to send data (recommended method when using React Router)

## 15. Working with action() Functions

1. in `App.js`, add the special `action` property to the `NewEventPage` route definition
2. just like `loader`, the `action` property wants a function
3. add the `action` function in `NewEvent.js`
4. in this `action` function send a POST request to the backend with the `fetch()` function
5. extract the data from that form
   1. make sure that all the inputs have the `name` attribute
   2. replace the `<form>` element by the special `<Form>` component provided by `react-router-dom`
   3. add to it the `method` property & set it to `post`
   4. in `NewEvent.js`, get hold of that request that is captured by the `<Form>` tag & forwarded to the `action` property with the `request` property & the `formData()` method & the `get()` method
6. add the data that was submitted with the form to the request via the `body` property
7. in `App.js`, import this `action` function for this `NewEventPage` route definition & use it as a value for this `action` property
8. in `NewEvent.js`, navigate the user away after submitting successfully the form with the `redirect` function

## 16. Submitting Data Programmatically

1. in `EventForm.js`, you could send the request to a different route with the `action` attribute
2. a different way of triggering an action
   1. in `EventItem.js`, inside of the `startDeleteHandler` function, show a confirmation prompt with the `window.confirm` function
   2. if it returns `true`, trigger an action that deletes this event
   3. in `App.js`, add an `action` property to the `EventDetailPage` route
   4. in `EventDetail.js`, add the `action` function
   5. in `EventItem.js`, don't trigger this action with the special `<Form>` component so you can display the prompt
   6. instead, trigger the action & submit the data programmatically with the special `useSubmit` hook imported `react-router-dom`

## 17. Updating the UI State Based on the Submission Status

1. in `backend\routes\events.js`, add a short delay before sending the post request
2. restart the backend server
3. when adding a new event, the submission will take a short while & we don't get any feedback regarding that
4. in `EventForm.js`, get some feedback & disable the `Save` button after submission with help of the `useNavigation` hook
5. in `backend\routes\events.js`, remove the `setTimeout()` function & restart the backend server

## 18. Validating User Input & Outputting Validation Errors

1. in `EventForm.js`, show the users any validation errors for example because some user disabled client side validation
2. in `NewEvent.js`, react to potential backend validation errors (status 422)
   1. don't show your default error page & throw an error response,
   2. instead, show the validation errors above the `<EventForm>` component so that you don't discard the values entered by the users
   3. to do that, return the data (`response`) you want to output above the form in the `action` function
   4. use the returned `action` data in `EventForm.js` with help of the `useActionData` hook

## 19. Reusing Actions via Request Methods

1. support submission for the `EditEvenPage` component
2. in `App.js`, register an `action` for this `EditEventPage` route
3. to do that, reuse the `action` which you are using for the `NewEventPage` route
   1. move the `action` function from `NewEvent.js` into `EventForm.js`
   2. make the code in this `action` function a bit more dynamic to be able to send both a request for adding a new event as well as for editing an existing event
   3. set a `method` prop
   4. in `NewEvent.js`, set the `method` to `post` & in `EditEvent.js`, set the `method` to `patch`
   5. in `EventForm.js`, extract the `method` with the `request` object & use it for the `method` of the request we are sending to the backend
   6. make the URL in the `action` function dynamic in `EventForm.js`
   7. in `App.js`, make sure this `action` that you could name `manipulateEventAction` is used on all the routes wheren you need to use it
   8. so add this `manipulateEventAction` to the `NewEventPage` & `EditEventPage` routes

## 20. Behind-the-Scenes Work with useFetcher()

1. add all the new provided files
2. trigger the `newsletterAction` whenever the `NewsletterSignup` form is submitted which is part off all pages because it is part of the `MainNavigation`
   1. in `NewsletterSignup.js`, use the `useFetcher` hook imported from `react-router-dom`
   2. use the `fetcher.Form` component to still trigger an action (or loader) without initializing a route transition (so without navigating to the page where the action or the loader belongs)
   3. add the `action` attribute to the form and trigger `/newsletter`
   4. but, with `fetcher.Form` you make sure that you don't load the element that belongs to this route component
   5. get some feedback & update the UI with the fetcher's `data` & `state` properties & `useEffect()`

## 21. Deferring Data Fetching with defer()

1. go back to `backend\routes\events.js`, bring back a timer on the get route
2. in `Events.js`, load the page before the data is there by deferring loading
   1. grab the code inside of the `loader()` function & outsource it into a separate `loadEvents()` function
   2. in the old `loader()` function, use the `defer` function imported from `react-router-dom`
   3. use the `Await` component imported from `react-router-dom` instead of the `<EventList>` component you were rendering
   4. set to it the `resolve` prop & pass to it the `data` deferred value which we named `events` in the `defer` function
   5. between the `<Await>` tags, output a dynamic value which must be a function that will be executed by React once that promise resolves
   6. wrap the `<Await>` component with the `<Suspense>` component imported from `react` to show a fallback whilst we're waiting for other data to arrive (in this case for these events to be fetched)
   7. don't return the `response` anymore inside of the `loadEvents()` helper function, but parse it manually

## 22. Controlling Which Data Should Be Deferred

1. in `EventDetail.js`, besides the `<EventItem>` component, output the `<EventsList>` component
2. fetch the `events` on this page
   1. copy the `loadEvents()` from `Events.js` & paste it in `EventDetail.js`
   2. add a new `loadEvent()` function where you pass an `id` as an argument & paste inside of it the code from the `loader` function
   3. use these 2 helper functions inside of the `loader()` function to defer again with help of the `defer()` function
   4. use the `event` & `events` keys set in the `defer()` function with the `useRouteLoaderData()` function to get the data of these 2 defer requests so to say
   5. use 2 `<Await>` components to wrap every component you want to render to await these 2 different requests
   6. use 2 `<Suspense>` components to wrap every `<Await>` component
3. this is still not the perfect solution because you can sometimes see the 2 loading messages of both deferred pieces of data
   1. tell React Router to wait with displaying the `EventDetailPage` until the details have been loaded
   2. it should then load the `EventsList` component after we navigated to the `eventDetailPage`
   3. to do so, in `EventDetail.js`, add the `await` keyword before `loadEvent(id)` inside the `defer` function
   4. with that, you will never see the "Loading..." message for the event detail when navigating to the `EventDetailPage`
4. side note: in `EventsList.js`, convert the links into absolute paths
