# ![[tktk Module Name] - Level Up - Viewing a User's Favorite Listings](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to create a profile page for displaying data related with a user.

## User story

- AAU viewing the navbar, I should be able to follow a link to my profile page where I can view the listings I own and the listings that I have favorited.

## Conceptualizing the route

Our route should listen for `GET` requests on `/users/profile`. 

| Action | What It Does | HTTP Verb | Route |
| ------ | ------------ | --------- | ----- |
| Profile | Displays a page with the listings owned and favorited by a user | `GET` | `/users/profile` |

> 💡 We could include the `userId` as a parameter in our route, but in this case we'll rely on our auth middleware for identifying the necessary resources.

## Building the UI

Add the following anchor tag to the navbar:

```html
<!-- views/partials/_navbar.ejs -->
<a href="/users/profile">Profile</a>
```

## Scaffolding the function

Before we can build the scaffolding for this functionality, we'll need to run through some additional setup.

### Setup

First let's create and mount a new `usersController` file.

Run the following command in your terminal:

```bash
touch controllers/users.js
```

And add the following boilerplate:

```javascript
// controllers/users.js
const express = require('express');
const router = express.Router();
const Listing = require('../models/listing.js');

module.exports = router;
```

Import the `usersController` in `server.js`:

```javascript
// server.js
const usersController = require('./controllers/users.js')
```

And mount the controller:

```javascript
// server.js
app.use('/users', isSignedIn, usersController);
```

### Defining the function

Next, let's build the scaffolding for our controller function. With this, we'll confirm that our route is working as expected, and that we have access to `user` through `req.session`.

Add the following:

```javascript
// controllers/users.js
router.get('/profile', async (req, res) => {
  try {
    console.log('user: ', req.session.user);
    res.send('Profile page');
  } catch (error) {
    console.log(error);
  }
});
```

In your browser, navigate to the profile page and check your VS Code terminal.

## Writing the controller action

After confirming that our route is working correctly, we should be ready to build out the controller action.

Despite this functionality being in `controllers/users.js`, the resource we are concerned with here is `Listing`. In our controller action, we want to `find()` and `render()` two kinds of listings:

1. `myListings`: Listings **owned** by the current user.
2. `myFavoriteListings`: Listings **favorited** by the current user.

We'll also `populate()` the `owner` path on both of these queries in case we want to display `owner.username` beside each listing.

Add the following:

```javascript
// controllers/users.js
router.get('/profile', async (req, res) => {
  try {

    const myListings = await Listing.find({
      owner: req.session.user._id
    }).populate('owner');

    const myFavoriteListings = await Listing.find({
      favoritedByUsers: req.session.user._id
    }).populate('owner');

    console.log('myListings:', myListings)
    console.log('myFavoriteListings:', myFavoriteListings)
    
    res.send('Profile page'); 
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```

> 💡 Notice how we are filtering the results of the `find()` operation by values held in an array field with `favoritedByUsers: req.session.user._id`.

In your browser, once again navigate to the profile page and check your VS Code terminal. You may need to create or favorite a listing if you have not done so already.

After verifying the data, be sure to render and pass `myListings` and `myFavoriteListings` to the view:

```javascript
    res.render('users/show.ejs', {
      myListings, myFavoriteListings
    });
```

## Responding with a view

Our last step here is to create and build out the profile page.

Run the following command in your terminal:

```bash
touch views/users/show.ejs
```

Start by adding the following:

```html
<!-- views/users/show.ejs -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Profile</title>
  </head>
  <body>
    <%- include('../partials/_navbar.ejs') %>
    <h1>Profile</h1>

    <h2>My Listings</h2>

    <h2>My Favorites</h2>
    
  </body>
</html>
```

### Displaying the listing count

Notice how we've marked off two distinct sections of the profile page with these `<h2>` tags. One will be used for displaying listings the user owns, and the other for displaying listings the user has favorited. 

Below each `<h2>`, let's include a `<p>` tag that displays how many corresponding listings are in that section.

Add the following to the 'My Listings' section:

```html
<!-- views/users/show.ejs -->
    <h2>My Listings</h2>
    <p>
      You own
      <%= myListings.length %> 
      <%= myListings.length === 1 ? 'listing.' : 'listings.' %> 
    </p>
```

> 💡 The ternary above is used to adjust the text between 'listing' and 'listings' based on the number of items in `myListings`. This ensures that our messaging is grammatically correct.

**You Do:**

Using the above code as reference, add a `<p>` tag to the 'My Favorites' section that notifies the user of the number of listings that they have favorited.

### Rendering the listings

Next, add the following `<ul>` to the 'My Listings' section:

```html
<!-- views/users/show.ejs -->
    <ul>
      <% myListings.forEach((listing) => { %>
        <li>
          A listing in <%= listing.city %> owned by <%= listing.owner.username %>.
          <a href="/listings/<%= listing._id %>">View more details.</a>
        </li>
      <% }) %>
    </ul>
```

**You Do:**

Using the above code as reference, render each item in `myFavoriteListings`.