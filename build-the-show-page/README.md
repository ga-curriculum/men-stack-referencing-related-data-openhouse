# ![[tktk Module Name] - Build the Show Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to build the functionality for displaying a single resource on its own page.

## User story

Now that we are displaying all the listings on our index view, let's take a look at the next user story:

- I want to view all the details of a listing, including its owner, on a new page by following a link from that index page.

Here we'll want to add functionality for displaying a single listing on its own page. 

## Conceptualizing the route

Our route should listen for `GET` requests on `/listings/:listingId`.

| Action | What It Does | HTTP Verb | Route |
| ------ | ------------ | --------- | ----- |
| Show | Display a single listing | `GET` | `/listings/:listingId` |

## Building the UI

Next we'll need to make a small change in `views/listings/index.ejs`. The UI that issues our `GET` request will be an anchor tag that prompts the user to view additional details on a particular listing. We'll also need to include the `listing._id` in the request URL. This will allow us to retrieve a specific listing in our controller function.

Update the `<li>` with the achor element as shown below:

```html
<!-- views/listings/index.ejs -->
<li>
  A listing in <%= listing.city %> owned by <%= listing.owner.username %>.
  <a href="/listings/<%= listing._id %>">View more details.</a>
</li>
```

## Scaffolding the function

Next, let's build the scaffolding for our controller function. 

This scaffolding will help us confirm a few things:

1. The route is responding to the request issued by the anchor tag in `views/listings/index.ejs`.
2. Our function has access to the `listingId` included in `req.params`. being passed from the front end.

Add the following to `controllers/listings.js`:

```javascript
// controllers/listings.js
router.get('/:listingId', async (req, res) => {
  try {
    console.log('listingId: ', req.params.listingId);
    res.send(`Listings show page`);
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```

After completing the steps above, click on the 'View more details' link on the listings landing page. In your browser, you should see the 'Listings show page' message. In your terminal, you should see the `ObjectId` for the listing that you clicked on.

## Writing the controller action

Now that our function has access to the `listingId`, we can retrieve a specific listing and render it to the view.

Based on our user story, we know we'll need to include information on the listing owner, meaning we'll need to utilize the `populate()` method once again.

Update the function as shown below:

```javascript
router.get('/:listingId', async (req, res) => {
  try {
    
    const populatedListings = await Listing.findById(
      req.params.listingId
    ).populate('owner');

    res.render('listings/show.ejs', {
      listing: populatedListings,
    });

  } catch (error) {
    console.log(error);
    res.redirect('/');
  }
});
```

> 💡 In our application, we'll need to `populate()` the `owner` path anytime we wish to display information about an `owner` beyond their `ObjectId.`

## Rendering the view

Let's outline some of the things we'll need to accomplish in our show view. 

We'll want two sections here:

- A **details section** with information on the listing, including its `city`, `size`, `price`, and `streetAddress`.

- An **owner section** section displaying the `username` of the listing `owner`, with a message indicating whether the user viewing the listing is the owner of the property.

Run the following command in your terminal:

```bash
touch views/listings/show.ejs
```

Let's address the boilerplat and listing details section first. 

Add the following to the view:

```html
<!-- views/listings/show.ejs -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title><%= listing.name %></title>
  </head>
  <body>
    <%- include('../partials/_navbar.ejs') %>

    <h1>A listing in <%= listing.city %></h1>

    <h2>Details</h2>
    <p>House Size: <%= listing.size %> sq ft</p>
    <p>Price: $<%= listing.price %></p>
    <p>Street address: <%= listing.streetAddress %></p>

  </body>
</html>
```

Next, let's add a section for information on the owner. We'll make use of some conditional rendering here.

Add the following to the view:

```html
<!-- views/listings/show.ejs -->
    <h2>Owner</h2>
    <% if (listing.owner._id.equals(user._id)) { %>
      <p>You own this!</p>
    <% } else { %>
      <p>You don't own this, <%= listing.owner.username %> does!</p>
    <% } %>
  </body>
```