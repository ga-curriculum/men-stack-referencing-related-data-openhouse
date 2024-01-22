# ![tktk Module Name - Wrap Up](./assets/hero.png)

**Learning Objective:** By the end of this lesson, students will understand the planning and functionality essential for building this application.

## Recap

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