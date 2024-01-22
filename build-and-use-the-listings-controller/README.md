# ![MEN Stack Referencing Related Data - RealApp - Build and Use the Listing Controller](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## Building the listings controller

We are still on the user story:

- As a user, I want to create a listing for my own real estate.

However, we still need to do some prep work before we can create a listing. We have our model now. Let's create our controller. The goal here is to create a router, import the listings model, and export the router so that we can mount it in our `server.js` file.

Let's create a new file in our `controllers` directory called `listings.js`. Inside `listings.js` let's set up our router, require in our listing model, and export our router:

```js
const express = require('express');
const router = express.Router();

const Listing = require('../models/listing');

module.exports = router;
```

Once our router is exported, we can import it into our `server.js` file and mount it:

```js
// other require statements for libraries above
const authController = require('./controllers/auth.js');
const listingsController = require('./controllers/listings'); // add listings controller

// Express application

app.use('/auth', authController);
app.use('/listings', listingsController); // mount listings controller
```
