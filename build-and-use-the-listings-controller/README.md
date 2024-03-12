# ![MEN Stack Referencing Related Data - RealApp - Build and Use the Listing Controller](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## Building the listings controller

We are still on the user story:

- As a user, I want to create a listing for my own real estate.

However, we need to do some more prep work before we can create a listing. We have our model now, so let's create our controller. The goal here is to create a router, import the listings model, and export the router so that we can mount it in our `server.js` file.

Let's create a new file in our `controllers` directory called `listings.js`. Inside `listings.js` let's set up our router, require in our listing model, and export our router:

```js
// controllers/listings.js

const express = require('express');
const router = express.Router();

const Listing = require('../models/listing');

module.exports = router;
```

Once our router is exported, we can import it into our `server.js` file and mount it:

```js
// server.js

// other require statements for libraries above
const authController = require('./controllers/auth.js');
// add listings controller:
const listingsController = require('./controllers/listings'); 

// . . .

app.use('/auth', authController);
// mount listings controller
app.use('/listings', listingsController); 
```

Great, our controller file is set up. We won't set up any routes yet, but we'll be returning to build them out soon! 