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
