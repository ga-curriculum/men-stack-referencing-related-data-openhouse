# ![[tktk Module Name] - Build the Listings Landing Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## Route for GET All listings

Before we can create a listing we are going to need a route to display all of the listings. This way we can redirect to this listings page after a user creates a new listing. Let's start by thinking of this routes to get all the listings. We can determine that we need a `GET` request to `/listings`. This will be our index route for listings.

| Action | What It Does | HTTP Verb | Route |
| ------ | ------------ | --------- | ----- |
| Index | Displays all listings | `GET` | `/listings` |

> The index action is one of two read action in CRUD. It is used to display all of the resources of a given type.

## You Do: Log all listings

We have determined that we need a route for `GET` requests to `/listings`. Let's build it! Inside of `controllers/listings.js` do the following steps:

1. Create a route for `GET` requests to `/listings`.
2. Using the `Listing` model, find all listings.
3. `console.log()` the listings.
4. `res.send()` a message that says "Listings index page".
5. `console.log()` any errors that occur.
6. Test your route in the browser by going to `localhost:3000/listings` manually.

> Make sure you have started your server with `nodemon` before testing your route.

After you have completed the above steps you should be able to see the message "Listings index page" in the browser. You should also see an empty array in the terminal. This is because we have not created any listings yet.

Make sure you have debugged your application before moving on.

## Render Listings Index Page

We have confirmed that our route is working, now it's time to render the listings index page. For right now we are not going to worry about handling any data, we just want to set up a place that we can add to in the future. Let's create a new directory in our `views` directory called `listings`. Inside of `views/listings` create a new file called `index.ejs`. Inside of `index.ejs` let's add the following code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>All Listings</title>
</head>
<body>
    <h1>Here's all the Listings</h1>
    
</body>
</html>
```

> VS Code allows us to add biolerplate HTML by typing `!` and hitting the `tab` key.

Now we have an index page for our listings. Let's render this page in our route. Inside of `controllers/listings.js` let's update our route to render the listings index page:

```js
// located inside the index route
res.render('listings/index.ejs');
```

tktk Notes: Build the Listings landing page. Do not pass or display any data on this page right now - we only want to set up a place for the user to start interacting with this resource. No user story here, but a necessary step on the path. Don't build a UI to go to this page, have students manually go here for now. We'll fix that in the nav bar partial microlesson next.

tktk App flow: We build each route with the following process:

1. Conceptualize the route. No code is written here, all we are doing is deciding what our route should be to make future steps simpler.
2. Build the UI (link or form) that will hit that route. Send any relevant data on the request. At this point the route should not function! This is a great prompt to move students into the next step:
3. Scaffold the function that corresponds that that route. `console.log()` any relevant data (for example: `req.params`, `req.body`, etc) so we know what we have access to in the function and the shape of the data we're receiving from the front end. `res.send()` (not `res.render()`) a very brief message identifying the page so there is visual confirmation for students that the route worked. Have students thoroughly test and debug at this stage. Avoid code that does more than logging and doing the `res.send()` action for now.
4. Write controller action. After confirming the route and data are accurate and accounted for, the rest of the controller action should be crafted.
5. In the case of a GET request, respond with a view (you may need to build the view in this step, that is expected). Any other kind of request should redirect somewhere.

tktk This follows the flow of data into and out of our server, ensuring students can test their work along the way and view data when necessary.

tktk Code: The final code for this lecture can be found at: <https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-referencing-related-data-real-estate-solution>
