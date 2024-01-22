# ![tktk Module Name - Wrap Up](./assets/hero.png)

**Learning Objective:** By the end of this lesson, students will understand the planning and functionality essential for building this application.

## Project Planning Review - Real Eastate App

We have built a real estate application that allows users to create listings for their own real estate. Details they can track are:

- Street address
- City
- Price
- Size in sq ft
- Owner
- Users who have favorited the listing

Users can browse all listings, access listing details, create new listings, edit existing ones, and delete listings. Additionally, users can favorite or unfavor a listing.

We built out our user stories and ERD to help us plan out this application. We also built out our application in a way that we can easily add new features to it. 

### User Stories

User stories are a way to describe an application's functionality from a user's perspective. Remember, they are often written in the format of:

> As a user, I want to do X.

User stories are often used as a checklist to work through as we build out an application. 

### ERD

Enitiy relationship diagrams (ERDs) are a way to visualize the structure of the data in an application. They are often used to plan out the structure of an application's database.

tktk - Hunter - once the ERD is finalized, add it here too

## Referencing one-to-many Relationships

We have learned how to reference one-to-many relationships in our application. We have done this by adding a reference to the `owner` field in our `listings` collection. This allows us to track which user owns a listing.

The process of referencing one-to-many relationships is:

1. Add a field to the Schema of the many side of the relationship.

```js
const listingSchema = new Schema({
    // other fields
    owner: {}
});
```

2. Add a reference to the Schema of the many side of the relationship.

```js
const listingSchema = new Schema({
    // other fields
    owner: {
        type: Schema.Types.ObjectId,
        ref: 'User'
    }
});
```

> We use the `ref` key to tell Mongoose which collection to use for the reference. In this case, we are referencing the `User` collection.

Lastly we could have embedded the `listingSchema` inside of the `userSchema` to accoplish much the same relationship. This would still be a one-to-many relationship. However, we have choosen to reference the `User` collection from the `Listing` collection.


tktk Notes: Review what we built, how we planned out this app, and how that planning helped us as we built this out. Review the key concepts of referencing and M:M relationships. Review how we protected resources from being modified by people other than those that created them. Celebrate building out this large app!

Congratulations! You've just completed building an Express application where data relationships are managed through referencing.

Let's take a moment to review what we've achieved...

## Implementing Many-to-Many Relationships

We learned how to implement many-to-many (M:M) relationships through our application's favoriting feature.

In an M:M relationship, multiple documents from one collection associate with multiple documents from another collection.
In our application, users can favorite multiple listings and a listing can be favorited by many users.
The `favoritedByUsers` property in `listingSchema` stores an array of user references.
Utilized the `populate()` method for data retrieval.

## Protecting Resources

We also focused on securing resources, ensuring only authorized users can modify them.
Resource protection restricts access to functionalities based on specific user criteria.
We ensured resources like listings can only be modified by their owners.
To do so, we included conditional rendering in the client-side interface, and protected controller actions on the server.