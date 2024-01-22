# ![MEN Stack Referencing Related Data - RealApp - Level Up - Accounting for User Preferences](./assets/hero.png)

tktk Notes: If there is time to build this (stretch goal 2), as a level up activity it would be cool if we could accomodate global users of this app and display the size of a listing using imperial (sq ft) or metric (sq meter) units based on a user's preferences. A few topics could be covered as part of this:

- To avoid having to create a new page to change a single preference, this functionality might be best held in the nav bar.
- We need to decide on how to access this information when a request is made. Quickly assessing this, it's probably easiest to access on the session, which will mean updating that and the db when the user changes their preference, as well as ensuring this is attached to the session with a user signs in. It would be good to dive into the advantanges/disadvantages of whatever path is chosen here. 
- Considerations about how the data for the size of a listing is stored in a database and the implications of that decision. As part of this we should decide how to convey this in the lecture - current listings will likely be stored using sq ft so continuing with that as the default unit in the db would likely be the most straightforward approach.
- Considerations for how this impacts existing pieces of our application. We should endevor to respect the user's preference throughout our app therefore almost every existing page is impacted by this change. We should think about how to handle this preference in the existing new/edit forms (imo would be cool if we had radio buttons for units of measurement and pre-selected the one that matched their preferences, then manipulated the data in the controller based on the form data, since it's not something they see often.)
- This is a different perspective on accessibilty/usability that many students may not immediately consider as an accessiblity feature and has the potential to shift their mindset around this topic. This should absolutely be called out in this level up!
- There's probably other aspects of this I'm missing, but that seems like a good start.

There is alot here, but this seemingly small change in this app has lots of cool topics within it that we don't get to cover anywhere else, and I think would be a valuable and fun addition to this module.

tktk code: No final code for this.
