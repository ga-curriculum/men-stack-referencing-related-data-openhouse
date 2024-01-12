# ![[tktk Module Name] - Setting the Stage](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

tktk Notes: What're we building? Establish what this application is here. Discuss user stories. Go over an ERD here. We've already covered referencing data a bit in <https://git.generalassemb.ly/modular-curriculum-all-courses/mongoose-relationships>, but a quick reminder of the concepts covered there would be good. It would also be good to review 1:M and M:M relationships, and their significanced. Could potentially reference patterns described in <https://www.mongodb.com/developer/products/mongodb/mongodb-schema-design-best-practices/> to assist. Continue to reference user stories and the ERD throughout this lecture where appropriate.

tktk Here's some user stories to get going, feel free to flesh these out, they're just quick starting points:

tktk - As a user (AAU), I should be able to create real estate listings for real estate that I own. I should be able to keep track of the street address, the city it is located in, its listing price, and the size of the listing in sq ft. [tktk Side node, I (David) think this schema is largely fine but I do not like `size` - if you think of a better name for this please feel free to change it, keep in mind the **Accounting for User Preferences Level Up** as you're thinking through names for this, happy to be a sounding board for this too.]

tktk - AAU, I should be able to view all the listing that have been created by every user on a single page. Only the city and owner for each listing should be listed here to keep this view digestable. [tktk When CSS is developed, this view should be fleshed out some to have more comprehensive information, but keep it simple for the initial build so students can move through this quickly to get going]

tktk - AAU, I should be able to view all the details of a listing, including its owner, on a new page by following a link from that index page.

tktk - AAU viewing the details of a listing that I own, I should be able to click a button and delete the listing.

tktk - AAU viewing the details of a listing that I own, I should be able to follow a link to be taken to a page where I can edit any of the details of a listing and update it from there.

tktk - AAU viewing the details of a listing, I should be able to see how many people have favorited that listing, and be able to favorite it myself.

tktk - AAU viewing the details of a listing that I have favorited, I should be able to unfavorite the listing.

tktk it might also be good to draw student's attention to how with just these user stories we can get an idea of what the schema of the applications resource will look like, along with what CRUD actions users will be able to carry out on that resource.

tktk here's a demo ERD, this should be replaced when assets are created, but it's a start:

![Job Applications ERD](./assets/tktk-erd-placeholder.png)

tktk Hunter, can you fix up this ERD?

tktk Code: The final code for this lecture can be found at: <https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-referencing-related-data-real-estate-solution>
