# ![MEN Stack Referencing Related Data - RealApp - Build Update Functionality](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## User Story

Now that we are rendering an edit view, we can address the next component of this user story:

- AAU viewing the details of a listing that I own, I should be able to follow a link to be taken to a page where I can edit any of the details of a listing and update it from there.

As with our delete functionality, we'll want some additional security here. The edit link is only visible to the owner of the listing, but an unauthorized user could still find a way to navigate to the edit page. To handle that scenario, we'll make sure the controller action is protected. Should a user attempt to update a listing they don't own, they will receive a message: 'You don't have permission to do that.'

> 📚 The term **protected controller action** refers a controller action that is secured through an authentication or authorization check. This means that only users who meet certain criteria can access the functionality of that action.

## Conceptualizing the route

Our route should listen for `PUT` requests on `/listings/:listingId`.

| Action | What It Does | HTTP Verb | Route |
| ------ | ------------ | --------- | ----- |
| Update | Update a single listing | `PUT` | `/listings/:listingId` |

## Building the UI

Next we'll need to add the following `<form>` to our edit view:

```html
<!-- views/listings/edit.ejs -->
  <form action="/listings/<%= listing._id %>?_method=PUT" method="POST">
    <label for="streetAddress">Street address:</label>
    <input
      type="text"
      name="streetAddress"
      id="street"
      value="<%= listing.streetAddress %>"
    />
    <label for="city">City:</label>
    <input type="text" name="city" id="city" value="<%= listing.city %>" />
    <label for="price">Price:</label>
    <input
      type="number"
      name="price"
      id="price"
      value="<%= listing.price %>"
      min="0"
    />
    <label for="size">Size:</label>
    <input 
      type="number"
      name="size"
      id="size"
      value="<%= listing.size %>"
      min="0"
    />
    <button type="submit">Edit Listing</button>
  </form>
```

## Scaffolding the function

Let's build the scaffolding for our controller function. 

With this we'll confirm the following:

1. The `listingId` is correctly passed through `req.params`.
2. The `user` object is accessible via `req.session`.

Update `controllers/listings.js` with the code below:

```javascript
// controllers/listings.js
router.put('/:listingId', async (req, res) => {
  try {
    console.log('listingId:', req.params.listingId)
    console.log('user:', req.session.user)
    res.send(`A PUT request was issued for ${req.params.listingId}`)
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```

## Writing the controller action

Now that we have confirmed access to `req.params.listingId` and `req.session.user._id`, let's complete the controller action.

The controller action should accomplish the following:

1. Retrieve a specific listing using its `ObjectId`.
2. Verify permissions by comparing the `owner` property of the listing with `req.session.user._id`. If the user making the request is not the owner, they should receive the message: "You don't have permission to do that."
3. If the user is the owner, the listing should be updated using the `updateOne()` method.

### Retrieving the listing

First, let's retrieve the listing using the `findById()` method:

```javascript
// controllers/listings.js
router.put('/:listingId', async (req, res) => {
  try {
    const currentListing = await Listing.findById(req.params.listingId);
    res.send(`A PUT request was issued for ${req.params.listingId}`)
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```

### Implementing a permission check

Next, let's address the perission check.

Update the function with the following `if...else` block:

```javascript
// controllers/listings.js
router.put('/:listingId', async (req, res) => {
  try {
    const currentListing = await Listing.findById(req.params.listingId);

    if (currentListing.owner.equals(req.session.user._id)) {
      console.log("Permission granted")
    } else {
      console.log("Permission denied")
    }

    res.send(`A PUT request was issued for ${req.params.listingId}`)
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```

### Updating the listing

Let's take a moment to explore the [`updateOne()`](https://mongoosejs.com/docs/api/model.html#Model.updateOne()) method.

When `updateOne()` is called on a document instance, it updates the document with the provided data:

```javascript
const docInstance = await Model.findById(req.params.modelId);
docInstance.updateOne({ name: 'Alex' });
```

> 🧠 The `updateOne()` method can also be called on the model. In this context, it requires an object specifying the criteria to identify the document for update. We're not using this approach because we need to verify user permission before deletion.

Update the function as shown below:

```javascript
// controllers/listings.js
router.put('/:listingId', async (req, res) => {
  try {
    const currentListing = await Listing.findById(req.params.listingId);
    if (currentListing.owner.equals(req.session.user._id)) {
      await currentListing.updateOne(req.body);
      res.redirect('/listings');
    } else {
      res.send("You don't have permission to do that.");
    }
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```