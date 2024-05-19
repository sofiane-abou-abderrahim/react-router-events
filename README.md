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
