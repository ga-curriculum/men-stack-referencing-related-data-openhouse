# ![[tktk Module Name] - Build Create Functionality](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## POST a New Listing

The action for creating a new listing breaks down into two parts:

- Render a form for the user to enter the details of the listing.
- Using the form data create the listing in the database.

In the last section we completed the first of the above tasks in order to create a new listing. Now we are going to focus on the second task. We want to use the form data to create a new listing in the database. Since we are accepting data from the user we know that we need a `POST` route to `/listings`. Let's add this route to our routes table:

| Action | What It Does | HTTP Verb | Route |
| ------ | ------------ | --------- | ----- |
| Index | Displays all listings | `GET` | `/listings` |
| New | Displays a form to create a new listing | `GET` | `/listings/new` |
| Create | Creates a new listing in the database | `POST` | `/listings` |

> We carried over the index and new routes from the previous section to build onto this routes table.

Now we know what our route should be, let's update our form in `views/listings/new.ejs` to send a `POST` request to `/listings`:

```html
<form action="/listings" method="POST">
```

## You Do: Create a New Listing

We have determined that we need a route for `POST` requests to `/listings`. Let's build it! Inside of `controllers/listings.js` do the following steps:

1. Create a route for `POST` requests to `/listings`.
2. `console.log()` the data that is being sent from the form.
3. Using `res.redirect()` redirect to `/listings`.

Test by submitting the form in the browser.

> Make sure to use `req.body` to access the data from the form.

After you have completed the above steps you should be able to see the data from the form in the terminal. You should also be redirected to `/listings` in the browser.

## Database Insertion

We have the form data! However we are missing one last piece of the puzzle. If we look at our schema again we can see that we have the field `owner`. We intentionally left that out of the form because we don't want the user to be able to set the owner of a listing. We want the current signed in user to create a listing that is owned by them. 

We have access to the current signed in user on the `req.session` object. We can use this to set the owner of the listing. Let's update our route to create a new listing in the database:

```js
router.post('/', async (req, res) => {
    req.body.owner = req.session.user._id;
    await Listing.create(req.body);
    res.redirect('/listings');
});
```

> If we were to `console.log` the `req.body` object after we add the `owner` key we would see an object that directly matches our schema. Since this is the case we can feed the `req.body` object directly into the `Listing.create()` method.

## Review Questions

1. What is the difference between `req.body` and `req.params`?
2. What is the difference between `res.redirect()` and `res.render()`?

tktk Notes: Draw attention to this being invisible to the user, but discuss how it's still a necessary step in completing the creation user story.

tktk App flow: We build each route with the following process:

1. Conceptualize the route. No code is written here, all we are doing is deciding what our route should be to make future steps simpler.
2. Build the UI (link or form) that will hit that route. Send any relevant data on the request. At this point the route should not function! This is a great prompt to move students into the next step:
3. Scaffold the function that corresponds that that route. `console.log()` any relevant data (for example: `req.params`, `req.body`, etc) so we know what we have access to in the function and the shape of the data we're receiving from the front end. `res.send()` (not `res.render()`) a very brief message identifying the page so there is visual confirmation for students that the route worked. Have students thoroughly test and debug at this stage. Avoid code that does more than logging and doing the `res.send()` action for now.
4. Write controller action. After confirming the route and data are accurate and accounted for, the rest of the controller action should be crafted.
5. In the case of a GET request, respond with a view (you may need to build the view in this step, that is expected). Any other kind of request should redirect somewhere.

tktk This follows the flow of data into and out of our server, ensuring students can test their work along the way and view data when necessary.

tktk Code: The final code for this lecture can be found at: <https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-referencing-related-data-real-estate-solution>
