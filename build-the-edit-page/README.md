# ![[tktk Module Name] - Build the Edit Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## User Story

Let's address the next user story:

- AAU viewing the details of a listing that I own, I should be able to follow a link to be taken to a page where I can edit any of the details of a listing and update it from there.

Much like creating a new resource, this functionality will require two requests:

1. A `GET` to render the edit view.
2. A `PUT` to update the resource.

We will begin with the `GET` request that renders the view.

## Conceptualizing the route

Our route should listen for `GET` requests on `/listings/:listingId`.

| Action | What It Does | HTTP Verb | Route |
| ------ | ------------ | --------- | ----- |
| Edit | Renders a view for editing a specific listing | `GET` | `/listings/:listingId` |

## Building the UI

Next we'll need to add an anchor tag to our show view. Like our delete button, this link should only be visible to the owner of a listing.

Update the `if` block as shown below:

```html
// views/listings/show.ejs
    <% if (listing.owner._id.equals(user._id)) { %>
      <p>You own this!</p>
      <a href="/listings/<%= listing._id %>/edit">Edit this listing</a>
```

## Scaffolding the function

Next, let's build the scaffolding for our controller:

```javascript
// controllers/listings.js
router.get('/:listingId/edit', async (req, res) => {
  try {
    console.log('listingId: ', req.params.listingId);
    res.send(`Listings edit view`);
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```

In your browser, navigate to the edit page and check your VS Code terminal for the logged data.

## Writing the controller action

With access to the `listingId` we can build out the rest of the function:

```javascript
// controllers/listings.js
router.get('/:listingId/edit', async (req, res) => {
  try {
    const currentListing = await Listing.findById(req.params.listingId);
    res.render('listings/edit.ejs', {
      listing: currentListing,
    });
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```

## Rendering the view

Lastly, let's create our edit view.

Run the following command in your terminal:

```bash
touch views/listings/edit.ejs
```

Add the following to `views/listings/edit.ejs`:

```html
<!-- views/listings/edit.ejs -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Edit <%= listing.name %></title>
</head>
<body>
  <%- include('../partials/_navbar.ejs') %>
  <h1>Edit a Listing!</h1>

</body>
</html>
```