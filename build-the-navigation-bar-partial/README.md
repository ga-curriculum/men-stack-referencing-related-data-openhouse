# ![MEN Stack Referencing Related Data - RealApp - Build the Navigation Bar Partial](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## Navigation bar

We are about to build another couple of routes to handle the creation of a listing. Let's build a navigation bar so that we can easily navigate between the different pages of our application.

We are going to be leveraging partials to build our navbar. Partials are a way to break up our views into smaller pieces. This is an excellent way to keep our code DRY and make it easier to maintain. Create a new directory called `partials` in the `views` directory. Inside `partials`, create a new file called `_navbar.ejs`. Inside of `_navbar.ejs`, let's create the HTML for our navbar that includes anchor tags for the following routes:

- Sign Up
- Sign In
- Sign Out
- Listings

```html
<nav>
  <a href="/listings">View all Listings</a>
  <a href="/auth/sign-out">Sign Out</a>
  <a href="/auth/sign-up">Sign Up</a>
  <a href="/auth/sign-in">Sign In</a>
</nav>
```

Apart from this, the application is to ensure that we only show the navbar links relevant to the user. For example, if a user is signed in, we do not want to show the signup and sign-in links, nor do we want guest users to view our listings. We have sent the current user to our views using the `res.locals` object. We can use this to conditionally render our navbar links.

Back in our `_navbar.ejs` file, let's add some logic to conditionally render our navbar links:

```html
<nav>
  <% if (user) { %>
    <a href="/listings">View all Listings</a>
    <a href="/auth/sign-out">Sign Out</a>
  <% } else { %>
    <a href="/auth/sign-up">Sign Up</a>
    <a href="/auth/sign-in">Sign In</a>
  <% } %>
</nav>
```

## You Do: Add navbar to listings index page

We have created our navbar partial. Now, let's add it to our listings index page. Inside of `views/listings/index.ejs` include the navbar partial. Test this by manually going to `localhost:3000/listings` in the browser.

> 🚨 Make sure to use the correct path to the navbar partial.
