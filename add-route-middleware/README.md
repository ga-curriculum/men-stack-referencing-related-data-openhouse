# ![[tktk Module Name] - Add Route Middleware](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## Authentication Middleware - Protecting Listings Routes

We are still on the user story:

- As a user, I want to view all the listings that have been created by every user on a single page.

This is our last prep item before we can view all listings. We need to add some middleware to our application to protect this route from users who are not logged in, and set up an object to help with our authorization when we get to our edit and delete functionality.

Let's protect this route from guest users or users that are not signed in. We can do this by adding a middlewre function. Let's create a new directory in the root of our application called `middleware`. Inside of this direcotry create a new file called `is-signed-in.js`.

Inside of `is-signed-in.js` let's create a middleware function that checks if a user is signed in. If they are signed in we will call `next()` to move on to the next middleware function. If they are not signed in we will redirect them to the sign in page.

First we need a function that will accepted the `req`, `res`, and `next` parameters. This is the standard Express signature for middleware functions. We are going to use the function name of `isSignedIn`:

```js
const isSignedIn = (req, res, next) => {};
```

Next we need to handle is a user is signed in and if they are not signed in. If we look back over in `server.js` we can see that we have saved the current signed in user to the `req.session` object. This is wonderful because now we can use that is an `if` condtion to check if the user is signed in. If they are signed in we will call `next()` to move on to the next middleware function.

```js
const isSignedIn = (req, res, next) => {
  if (req.session.user) return next();
};
```

Awesome sauce now we just have to handle for a user not being signed in. If a user is not signed in we want to redirect them to the sign in page. We can do this with the `res.redirect()` method. We will pass in the path to the sign in page as an argument to `res.redirect()`:

```js
const isSignedIn = (req, res, next) => {
    if (req.session.user) return next();
    res.redirect('/auth/sign-in');
};
```

Finally we have created this middleware function, but we need to export it so that we can use it in our application. At the bottom of `is-signed-in.js` let's export our middleware function:

```js
module.exports = isSignedIn;
```

Over in `server.js` let's import our middleware function let's use the `isSignedIn` middleware function to protect our listings routes. We can do this by adding it as the second argument to the `app.use()` method call for our listings controller:

```js
// other requrie statements for librarys above

const isSignedIn = require('./middleware/is-signed-in.js'); // require in middleware

const authController = require('./controllers/auth.js');
const listingsController = require('./controllers/listings.js');

// Express application

app.use('/auth', authController);
app.use('/listings', isSignedIn, listingsController); // add middleware to protect listings routes
```

## Autherization Middleware - Listing Owner

We have protected our listing routes from guest users or users that are not signed in so let's now set up an object to help with our authorization when we get to our edit and delete functionality. We know that we have access to the current signed in user on the `req.session` object. We can use this to either do some checks in our controller or we can assign the current signed in user to the `res.locals` object for checks in our views.

Both are valid options however we are going to go with the later because we also want to use the user in the views for dynamic visuals like saying hello to the user. Those two reasons are enough to go with the `res.locals` object.

In order to do this we need to create a new middleware function that will assign the current signed in user to the `res.locals` object. Let's create a new file in our `middleware` directory called `pass-user-to-view.js`. Inside of `pass-user-to-view.js` let's create a middleware function that follows the standard Express signature for middleware functions. We are going to use the function name of `passUserToView`:

```js
const passUserToView = (req, res, next) => {};
```

We know we need to assign the current signed in user to the `res.locals` object but what if there is no signed in user? We need to handle this case as well. Since we are wanting to do an action based on a boolean condition we can use a ternary expression to accomplish our task. If there is a signed in user we will assign the current signed in user to the `res.locals` object. If there is not a signed in user we will assign `null` to the `res.locals` object:

```js
const passUserToView = (req, res, next) => {
  res.locals.user = req.session.user ? req.session.user : null;
  next();
};

module.exports = passUserToView;
```

> Don't forget to use `next()` to move to the next action in our Express application and to export our middleware function!

Success! We now have a middleware function that will assign the current signed in user to the `res.locals` object so that we can use the `user` in our views. Let's import our middleware function into `server.js` and use it to assign the current signed in user to the `res.locals` object:

```js
// other requrie statements for librarys above

const isSignedIn = require('./middleware/is-signed-in.js');
const passUserToView = require('./middleware/pass-user-to-view.js'); // require in middleware

// Router/Controller require statements and database connection

app.use(
  session({
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: true,
  })
);

app.use(passUserToView); // add passUserToView middleware function to Express application

app.get('/', (req, res) => {
  res.render('index.ejs', {
    user: req.session.user,
  });
});

// Express application
```

> Using the `next()` method in Express is a way to move to the next action in our Express application. After we have assigned the current signed in user to the `res.locals` object we want to move on to the next action in our Express application. In this case that is the `app.get()` method call for the root route.

## Review Questions

1. What is the purpose of route middleware?
2. What is the difference between authentication and authorization?
3. What is the difference between `req.session` and `res.locals`?
