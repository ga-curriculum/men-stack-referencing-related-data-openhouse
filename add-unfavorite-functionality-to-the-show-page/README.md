# ![MEN Stack Referencing Related Data - RealApp - Add Unfavorite Functionality to the Show Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to implement functionality for unfavoriting a resource.

## User story

Take a look at the next user story. This should mirror the logic used to favorite a listing quite closely.

- AAU viewing the details of a listing that I have favorited, I should be able to unfavorite the listing.

## Conceptualizing the route

Our route should listen for `DELETE` requests on `/listings/:listingId/favorited-by/:userId`. 

| Action | What It Does | HTTP Verb | Route |
| ------ | ------------ | --------- | ----- |
| Unfavorite | Unfavorites a listing | `DELETE` | `/listings/:listingId/favorited-by/:userId` |

> 💡 We'll include the `userId` in our route to stay consistent with our functionality for adding favorites.

## Building the UI

Let's modify the 'Favorites' section of our show page with an 'Unfavorite it' button. As with adding favorites, we'll need to include `listing._id` and `user._id` in our `DELETE` request.

Add the following `<form>` to the `if` block:

```html
<!-- views/listings/show.ejs -->
    <h2>Favorites</h2>
    <p>Favorited by <%= listing.favoritedByUsers.length %> people.</p>
    <% if (userHasFavorited) { %>
      <p>You've favorited this listing!</p>

      <form
        action="/listings/<%= listing._id %>/favorited-by/<%= user._id %>?_method=DELETE"
        method="POST"
      >
        <button type="submit">Unfavorite it</button>
      </form>

    <% } else { %>
      ...
    <% } %>
```

## Scaffolding the function

Next, let's build the scaffolding for our controller function. With this we'll confirm that both the `listingId` and the `userId` are accessible through `req.params`.

Add the following to `controllers/listings.js`:

```javascript
// controllers/listings.js
router.delete('/:listingId/favorited-by/:userId', async (req, res) => {
  try {
    console.log('userId: ', req.params.userId)
    console.log('listingId: ', req.params.listingId)
    res.send(`Request to unfavorite ${req.params.listingId}`)
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```

In your browser, click on the 'Unfavorite it' button and check your VS Code terminal for the logged data.

## Writing the controller action

Now that we have confirmed access to `req.params.listingId` and `req.params.userId`, let's complete the controller action.

Our goal here is to update a listing by **removing** a `userId` from the `favoritedByUsers` array. Once again, we'll utilize the `findByIdAndUpdate()` method, but this time using the MongoDB *`$pull`* operator. 

The [*`$pull`*](https://www.mongodb.com/docs/manual/reference/operator/update/pull/) behaves much like the `$push` operator, except that it **removes** a target value from the array:

```javascript
const updateObject = { $pull: { targetArray: valueForRemoval } }
```

In this case, we'll want to remove `req.params.userId` from `favoritedByUsers`.

Update the function as shown below:

```javascript
// controllers/listings.js
router.delete('/:listingId/favorited-by/:userId', async (req, res) => {
  try {
    await Listing.findByIdAndUpdate(req.params.listingId, {
      $pull: { favoritedByUsers: req.params.userId },
    });
    res.redirect(`/listings/${req.params.listingId}`);
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```

In your browser, try unfavoriting a listing and verify that the UI changes as expected.