# ![[tktk Module Name] - Add Favorites Functionality to the Show Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

tktk Notes: Quite a few notes here, and there's some code to potentially flesh out as you approach this microlesson. The final code uses the user _id in the route here to establish that as a common pattern, even though we don't HAVE to do this. I don't feel strongly about this, but if we cut this we need to be very direct in the copy about why we don't need it here.
  
This could also technically be a PATCH request (and would probably be the more proper convention) but it would be the first time students would see that so we'd need to explain it here. I'm totally fine with that, I don't feel strongly either way personally, but I did go with this for simplicity. But if there is a place to do that, this is it.The route or controller will need to change to accommodate this if this is changed though.

Happy to discuss this further, again, I'm not attached to a particular convention here and so far there's not a demonstration of functionality like this in the curriculum so if there's thoughts we should get in a room and change this up.

Also this code:

```javascript
    const userHasFavorited = populatedListings.favoritedByUsers.some((user) =>
      user.equals(req.session.user._id)
    );
```

will need to be added to the show page controller function now. We could do this with another DB call and use MDB `$in` here, but since we have the data already I didn't do that. I didn't populate the `favoritedByUsers` field here because we don't really NEED to, but it might be good to do that here regardless just so that students see that pattern. 

All of that is to say that the implementation here works and is simple, but if there's any strong feelings about these details we should be open to change (everywhere of course, but especially here.)

tktk App flow: We build each route with the following process:

1. Conceptualize the route. No code is written here, all we are doing is deciding what our route should be to make future steps simpler.
2. Build the UI (link or form) that will hit that route. Send any relevant data on the request. At this point the route should not function! This is a great prompt to move students into the next step:
3. Scaffold the function that corresponds that that route. `console.log()` any relevant data (for example: `req.params`, `req.body`, etc) so we know what we have access to in the function and the shape of the data we're receiving from the front end. `res.send()` (not `res.render()`) a very brief message identifying the page so there is visual confirmation for students that the route worked. Have students thoroughly test and debug at this stage. Avoid code that does more than logging and doing the `res.send()` action for now.
4. Write controller action. After confirming the route and data are accurate and accounted for, the rest of the controller action should be crafted.
5. In the case of a GET request, respond with a view (you may need to build the view in this step, that is expected). Any other kind of request should redirect somewhere.

tktk This follows the flow of data into and out of our server, ensuring students can test their work along the way and view data when necessary.

tktk Code: The final code for this lecture can be found at: <https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-referencing-related-data-real-estate-solution>
