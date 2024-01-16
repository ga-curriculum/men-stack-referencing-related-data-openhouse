# ![[tktk Module Name] - Build the Listing Model](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## Build the Listing Model

We have already created the user model for our application, so we have already checked off three of our user stories:

- As a user, I want to create a new accout.
- As a user, I want to log in to my account.
- As a user, I want to log out of my account.

Let's focus on the next user story:

- As a user, I want to create a listing for real estate that I own.

Before we can create a listing we need to create a Schema and Model for our listing. Let's create a new file in our `models` directory called `listing.js`. Inside `listings.js` let's create a new Schema for our listing:

```js
const mongoose = require('mongoose');

const listingSchema = mongoose.Schema({});
```

tktk - waiting to for response on if missing the new keyword is a bug or not

First we require in Mongoose, then using the Schema class we create a new 

The Schema class takes in an object of key value pairs that represent the fields and data types of our model. Looking at our ERD for the listing model we can see that we need to track the following fields:

- Street address - String and is required
- City - String and is required
- Price - Number, minimum of 0, and is required
- Size - Number, minimum of 0, and is required

Translating this into code we get:

```js
const mongoose = require('mongoose');

const listingSchema = mongoose.Schema({
    streetAddress: {
        type: String,
        required: true,
    },
    city: {
        type: String,
        required: true,
    },
    price: {
        type: Number,
        required: true,
        min: 0,
    },
    size: {
        type: Number,
        required: true,
        min: 0,
    }
});
```

> The Schema class can take an optional second parameter that is also an object. This is where we can turn on options for a document such as timestamps.

## Embedding Vs. Referencing Data

In Mongoose there are two ways to represent relationships between data. The first is called embedding. This is where we nest one document inside another. The second is called referencing. This is where we store a reference to another document inside a document. 

In our ERD we have a one to many relationship between users and listings. While we could embed a listing schema inside of our user schema we are going to reference the user schema inside of our listing schema. This will allow us to easily check who owns a listing, and as a result we will be able to control who can edit and delete a listing.

Let's reference the User Model inside of our listing schema. First we will need a field to represent this relationship. We will call this field `owner`. This field will be a single object ID that will point to a single user.

```js
// ... other properties located above
size: {
        type: Number,
        required: true,
        min: 0,
    },
owner: {}
```

We will still need a `type` to be declared on this field. Since we are going to be holding object IDs we will need a special `type` that Mongoose provides called `mongoose.Schema.Types.ObjectId`. Now that we have the type that this field is going to hold we also need to tell Mongoose what model this field is referencing. We do this by adding the `ref` property to this field. In this case we are referencing the `User` model.

```js
// ... other properties located above
size: {
        type: Number,
        required: true,
        min: 0,
    },
owner: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'User',
    }
```

## Compile and Export the Listing Model

Now that we have created our listing schema we need to compile it into a model. We do this by calling the `mongoose.model()` method and passing in the name of the model we want to create, and the schema we want to use to create it.

```js
// ... listingSchema located above

const Listing = mongoose.model('Listing', listingSchema);

module.exports = Listing;
```

tktk Notes: Build the listing schema. Call out that we compile/export the schema as a model which differs from embedding. Reference the User story for creating a listing to help students see a pattern they can apply to their own project planning/building - User stories inform the ERD which informs the schema we build which in turn informs the shape of data as it flows through our app. `favoritedByUsers` should not be added at this time, it will be added in a future microlesson (**Add Favorites to the Listing Schema**).

tktk Code: The final code for this lecture can be found at: <https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-referencing-related-data-real-estate-solution>
