# ![[tktk Module Name] - Build the New Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## GET a New Listing Form

The action for creating a new listing breaks down into two parts:

- Render a form for the user to enter the details of the listing.
- Using the form data create the listing in the database.

These two part require seperate routes. Let's start to think through the first action. We want to render a form for the user to enter the details of their listing. From this we can determine that we need a `GET` route but we already have mapped a `GET` route to `/listings` to view all listings. Let's add `/new` to the end of the path to create a new unique route for this action.

| Action | What It Does | HTTP Verb | Route |
| ------ | ------------ | --------- | ----- |
| Index | Displays all listings | `GET` | `/listings` |
| New | Displays a form to create a new listing | `GET` | `/listings/new` |

> We carried over the index route from the previous section to build onto this routes table.

Now we know what our route should be, let's include an achor element in our `listings/index.ejs` file to link to this new route:

```html
<!--Meta Data located above-->
<h1>Here's all the Listings</h1>
<a href="/listings/new">Add a New Listing</a> <!--Add in new anchor element-->
<!--Closing body and html element under-->
```

## You Do: Render a New Listing Form

We have determined that we need a route for `GET` requests to `/listings/new`. Let's build it! Inside of `controllers/listings.js` do the following steps:

1. Create a route for `GET` requests to `/listings/new`.
2. Using `res.render()` render a new view called `listings/new.ejs`.
3. Inside of `views/listings` create a new file called `new.ejs`.
4. Include the navbar partial in `new.ejs`.
5. Add an `h1` element that says "Add a New Listing!".
6. Add a form that has a submit button. Leave form blank for now.

Test by using the anchor element in `listings/index.ejs` to navigate to `localhost:3000/listings/new` in the browser.

> Make sure to give your submit button text so you can see it in the browser.

After you have completed the above steps you should be able to see the message "Add a New Listing!" in the browser. You should also see the navbar partial and a submit button.

## Schema to Form Field Mapping

We are at the point of accepting some data from the user, however what should we accept? We have a schema for out listings, we will use it to determine what data we need to accept from the user. Let's take a look at our schema fields for listings:

```js
streetAddress: {
        type: String,
        required: true,
    },
    city: {
        type: String,
        required: true,
    },
    price: {
        type: Number,
        required: true,
        min: 0,
    },
    size: {
        type: Number,
        required: true,
        min: 0,
    }
```

We can see that we need to accept the following data from the user:

- Street address - String and is required
- City - String and is required
- Price - Number, minimum of 0, and is required
- Size - Number, minimum of 0, and is required

> We are intentionally leaving off the `owner` field because we are not going to expect our end users to enter their own user ID. We will handle this in the controller action.

We are going to need a form field for each of these fields. Let's add these to our form in `views/listings/new.ejs`:

```html
<form>
    <label for="street-address">Street Address:</label>
    <input type="text" name="streetAddress" id="street-address" />
    <label for="city">City:</label>
    <input type="text" name="city" id="city" />
    <label for="price">Price:</label>
    <input type="number" name="price" id="price" min="0"/>
    <label for="size">Size:</label>
    <input type="number" name="size" id="size" min="0"/>
    <button type="submit">Add Listing</button>
</form>
```

> The `name` attribute on the input elements is what we will use to access the data from the form in our controller action. We are also using some HTML form validation to make sure that the user enters a number greater than 0 for the price and size fields.

We have completed the first action of creating a new listing. We have rendered a form for the user to enter the details of their listing. Now we need to handle the data that is sent from the form.

tktk Notes: Draw a connection between the schema of the listings model and the details a user can enter on this page and reference back to the user story for creating a resource - while this doesn't fulfill that entire user story it is a necessary step on the path there. This is the first time they're seeing a number with some light validation in a form so it would be good to very quickly discuss that. 

tktk App flow: We build each route with the following process:

1. Conceptualize the route. No code is written here, all we are doing is deciding what our route should be to make future steps simpler.
2. Build the UI (link or form) that will hit that route. Send any relevant data on the request. At this point the route should not function! This is a great prompt to move students into the next step:
3. Scaffold the function that corresponds that that route. `console.log()` any relevant data (for example: `req.params`, `req.body`, etc) so we know what we have access to in the function and the shape of the data we're receiving from the front end. `res.send()` (not `res.render()`) a very brief message identifying the page so there is visual confirmation for students that the route worked. Have students thoroughly test and debug at this stage. Avoid code that does more than logging and doing the `res.send()` action for now.
4. Write controller action. After confirming the route and data are accurate and accounted for, the rest of the controller action should be crafted.
5. In the case of a GET request, respond with a view (you may need to build the view in this step, that is expected). Any other kind of request should redirect somewhere.

tktk This follows the flow of data into and out of our server, ensuring students can test their work along the way and view data when necessary.

tktk Code: The final code for this lecture can be found at: <https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-referencing-related-data-real-estate-solution>
