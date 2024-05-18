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

## 1. React Router Basics Refresher Practice Exercice

- Challenge / Exercise

1. run `cd frontend/ && npm install react-router-dom`
2. Add five new (dummy) page components (content can be simple `<h1>` elements)

   - `HomePage`
   - `EventsPage`
   - `EventDetailPage`
   - `NewEventPage`
   - `EditEventPage`

3. Add routing & route definitions for these five pages

   - `/ => HomePage`
   - `/events => EventsPage`
   - `/events/<some-id> => EventDetailPage`
   - `/events/new => NewEventPage`
   - `/events/<some-id>/edit => EditEventPage`

   1. in `App.js`, use `createBrowserRouter` imported from `react-router-dom`
   2. define the routes inside of this function
   3. use the `RouterProvider` component imported from `react-router-dom` to tell React which router should be loaded & rendered

4. Add a root layout that adds the `<MainNavigation>` component above all page components

   1. in the `components` folder, add a new `MainNavigation.js` file (one already exists in this project)
   2. in the `pages` folder, add a new `Root.js` file
   3. add a new root in `App.js` & render the `<RootLayout>` component
   4. add a special `children` property to wrap the other routes with this `<RootLayout>` component
   5. in `Root.js`, use the `Outlet` component imported from `react-router-dom` to define where these child routes should be rendered
   6. include the `<MainNavigation>` component above the `<Outlet>` component in `Root.js`

5. Add properly working links to the MainNavigation
6. Ensure that the links in MainNavigation receive an "active" class when active

   1. in `MainNavigation.js`, use the `NavLink` component imported from `react-router-dom`
   2. add the `className` prop to the `<NavLink>` component
   3. set a function to `className` where you pass to it the `isActive` property provided by `react-router-dom` to let the link active after we click on it
   4. add the `end` prop to the home link so that the CSS class applies only when the path ends with `/` and not when it begins with

7. Output a list of dummy events to the EventsPage
8. Every list item should include a link to the respective EventDetailPage

   1. add a list of events in `Events.js`
   2. add some links to the event details pages in `Events.js` with help of the `<Link>` component
   3. define a `EVENTS` array with dummy events
   4. render the list of events dynamically with the dynamic path

9. Output the ID of the selected event on the EventDetailPage

   1. in `App.js`, define a new router with a dynamic `path` segment/parameter with a `:eventId` as a placeholder & an `element` to render `<EventDetailPage>`
   2. in `EventDetail.js`, get hold of the actual value used instead of that `:eventId` placeholder with help of the `useParams` hook imported from `react-router-dom`

10. BONUS: Add another (nested) layout route that adds the `<EventNavigation>` component above all /events... page components
