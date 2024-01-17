# ![[tktk Module Name] - Setting the Stage](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

tktk - Does this flow correctly? Come back after first couples sections and review. Open questions: Is this to much? Should this be more here are the user stories/ERD let's go? Less let's plan this together and then go?

## Real Estate App

During this module we are going to create a real estate application. This application will allow users to create listings for real estate that they own. Details they will be able to track are:

- Street address
- City
- Price
- Size in sq ft
- Owner
- Users who have favorited the listing

Users will be able to view all listings, view the details of a listing, create a new listing, edit a listing, and delete a listing. Users will also be able to favorite a listing, and unfavorite a listing.

This application will also have user authentication and autherization. Users will need to sign up and log in to create, edit, or delete a listing. Users will also only be able to edit or delete a listing if they are the owner of that listing.

> A link to the fully built application can be found [here](#tktk-deployed-app-url).

## Project Planning

Project planning is key in any project large or small. This is an expected part of the development process in the real world, and it is a good habit to get into. It is also a good way to get a sense of the scope of a project, and to break it down into more manageable pieces.

Planning a project can look like a lot of things depending on the project, but there are some common elements that are good to include in any project plan. These are:

- User stories
- ERDs
- Wireframes

These elements can be created in any order, but it is often helpful to start with user stories, and then move on to ERDs and wireframes. User stories are a good way to get a sense of what the application will do, and what the user experience will be like. ERDs and wireframes are a good way to get a sense of what the data will look like, and what the user interface will look like.

### User Stories

User stories are a way to describe the functionality of an application from the perspective of a user. They are often written in the format of:

> As a user, I want to do X.

This is a good way to describe and breakdown the functionality of an application. It is also a good way to get a sense of what the user experience will be like. User story creation is often an iterative process, and it is common to start with a few user stories, and then add more as the project progresses. This being an iterative process there is often a way to track what is considered the minimum viable product (MVP) for an application. This is the minimum amount of functionality that an application needs to have to be considered a viable product.

Let's create some user stories for our real estate application.

As a user:

- I want to create a new accout.
- I want to log in to my account.
- I want to log out of my account.
- I want to view all the listings that have been created by every user on a single page.
- I want to create a listing for real estate that I own.
- I want to view all the details of a listing, including its owner, on a new page by following a link from that index page.
- I want to delete a listing that I own.
- I want to edit any of the details of a listing that I own.
- I want to favorite a listing.
- I want to unfavorite a listing.
- I want to see how many people have favorited a listing.
- I want to view all the listings that I have favorited on a single page.

> Sometimes you might see might see user stories written in the format of: As a user, I want to do X, beacuase of Y. This is a good way to add some context to a user story, but it is not necessary.

From creating these user stories we not only get a sense of what the functionality of our application will be, but we also now have a check list of sorts to work through. 

### ERDs

Entity relationship diagrams (ERDs) are a way to describe the data that will be used in an application. Using the user stories we created above we know that we need to keep track of users, and listings. Before thinking about the relationships between user and listings, let's think about what data we need to keep track of for each of these resources.

For users we need to keep track of:

- Username - String and is required
- Password - String and is required

For listings we need to keep track of:

- Street address - String and is required
- City - String and is required
- Price - Number, minimum of 0, and is required
- Size - Number, minimum of 0, and is required

tktk - Hunter can you make a ERD for this above data? No relationships yet, just the data.

> We are intentionally leaving off the favorites feature for now. We will add a field/value pair for this feature when we add realationships to our ERD.

Now let's add in the relationships. First let's dive into the relationship between users and listings. We want a user to be able to create, edit, and delete multiple listings they own. This is a one to many relationship. A user can have many listings, but a listing can only have one user.

We also have the favorites fuctionality. A user can favorite many listings, and a listing can be favorited by many users. This is a many to many relationship. To represent this in our ERD we will need to have a new field name to house the relationship. We will call this field `favoritedBy`.

tktk - Hunter (David) here's a demo ERD, this should be replaced when assets are created, but it's a start:

![Job Applications ERD](./assets/tktk-erd-placeholder.png)

### Wireframes

Wireframes are the final part to project planning. This is where you take your user stories and ERD and start to think about what the user interface will look like. Wireframes are often created in a tool like [Figma](https://www.figma.com/), but they can also be created on paper. The goal of wireframes is to get a sense of what the user interface will look like, and how the user will interact with it.

tktk - Do we even want this wireframe section? I know we are going to be building a full seperate module on styling and CSS for this unit but wireframes
