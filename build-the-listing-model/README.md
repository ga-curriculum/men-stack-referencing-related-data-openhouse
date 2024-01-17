# ![[tktk Module Name] - Build the Listing Model](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## Build the Listing Model

We have already created the user model for our application, so we have already checked off three of our user stories:

- As a user, I want to create a new accout.
- As a user, I want to log in to my account.
- As a user, I want to log out of my account.

Let's focus on the next user story:

- As a user, I want to view all the listings that have been created by every user on a single page.

Before we can create a listing we need to create a Schema and Model for our listing. Let's create a new file in our `models` directory called `listing.js`. Inside `listings.js` let's create a new Schema for our listing:

```js
const mongoose = require('mongoose');

const listingSchema = new mongoose.Schema({});
```

> You should always use the [`new`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) keyword when creating a new instance of a Schema. This is because we do not want to modify the Schema class itself, but instead we want to create a new instance of it. This is a common pattern in object oriented programming and is best practice.

First we require in Mongoose, then using the Schema class we create a new instance of a Schema. The Schema class takes in an object of key value pairs that represent the fields and data types of our model. Looking at our ERD for the listing model we can see that we need to track the following fields:

- Street address - String and is required
- City - String and is required
- Price - Number, minimum of 0, and is required
- Size - Number, minimum of 0, and is required

Translating this into JavaScript using the Mongoose library and we get:

```js
const mongoose = require('mongoose');

const listingSchema = new mongoose.Schema({
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

> The Schema class can take an optional second parameter that is also an object. This is where we can turn on options for a document such as timestamps. Also notice that we have kept camel case for our field names to stay consistent with the naming convention of JavaScript.

## Embedding Vs. Referencing Data

In Mongoose there are two ways to represent relationships between data. The first is called embedding. This is where we nest one document inside another. The second is called referencing. This is where we store a reference to another document inside a document. 

In our ERD we have a one to many relationship between users and listings. While we could embed a listing document inside of our user document we are instead going to reference a user document inside a listing document. This will allow us to easily check who owns a listing, and as a result we will be able to control who can edit and delete a listing.

Let's put this into practice. First we will need a field to represent this relationship. We will call this field `owner`. 

```js
// ... other properties located above
size: {
        type: Number,
        required: true,
        min: 0,
    },
owner: {}
```

This is still a field in Mongoose so we still have to give this field a type. However, we are not storing just a string or a number in this field. Instead we are going to store a users document object ID. There is a special type in Mongoose for this called `mongoose.Schema.Types.ObjectId`. We will also need to tell Mongoose what model this field is referencing. We do this by adding the `ref` property to this field. In this case we are referencing the `User` model.

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

> Notice we have not added the `favoritedBy` field yet. This field value pair will be added in a future mircolesson.

## Compile and Export the Listing Model

So far we have only created a schema for our listing. This is just a blueprint for what our listing will look like. In order to have access to methods that would allow us to create, read, update, and delete listings we need to compile our schema into a model. We do this by calling the `mongoose.model()` method and passing in the name of the model we want to create, and the schema we want to use to create it. Lastly because we want to use this model in other files we need to export it.

```js
// ... listingSchema located above

const Listing = mongoose.model('Listing', listingSchema);

module.exports = Listing;
```

That is it! We now have a model for our listing that we can use to create, read, update, and delete listings.

## Review Questions

1. What is the difference between embedding and referencing data?
2. What is the difference between a Schema and a Model in Mongoose?
