# Overview

Redux is one of the most popular state management libraries out there, its the defacto standard expecially when building React apps, in this aside we will look into the usafe of **selector** and why its a good practice to use them often when implementing the library in any project.

# Selectors in REDUX

*potential issue of connecting our state directly to our components*

![Redux_without_selectors](https://productioncoder.com/wp-content/uploads/2018/10/redux-without-selector.jpg)

Given that there are multiple components directly conneted to the Redux store, and we decide to change the name of a api property from *lastName* to surname, we have to make sure we touch all the components that are connexted to this particular part of the state.

![Redux_without_selectors_broken](https://productioncoder.com/wp-content/uploads/2018/10/redux-without-selector-broken.jpg)

The dashed lined symbolicay represent the broken connection in need of fixing. This issue becomes exponential the more components we have attached to the state. *Selectors* help in order to aleviate this problem.

# Selectors kick in

Slector are simply adding a middle man between our state and the component, this middle man is just a function that access our state directly.

![Redux_selector_no_caching](https://productioncoder.com/wp-content/uploads/2018/12/redux-selector-no-caching.jpg)

Now when we change *lastName* to *surname* what happens ?

![Redux_selector_no_caching_broken_connection](https://productioncoder.com/wp-content/uploads/2018/12/redux-selector-no-caching-broken-connection.jpg)

The only thing broken is the functionality that access the state directly. In this case its simply the *getLastName* function a.k.a our selctor.

By just leting our selctor access the state directly, we are reducing the coupling in our app significantly. As illustrated above, the only thing broken is the selector, since we changed the way how our global state is defined. Now if there is a need to refactor or change the state definition, it just boils down to adapting a few selectors and reducers. There is no need to touch all the components associated with using a particular part of the state.

*You should always strive to use selectors in your Redux application, because its makes it easier to maintain the code and extendable. You also dont spend alot of time refactoring your mapToStateProps functions*

# Adding more spice (intro to Redux-reselect)

Having selectors is nice, but this approach can be taken up a notch with [Reselect](https://github.com/reduxjs/reselect), its a package that allows you to do performance optimization with Redux state. [Reselect](https://github.com/reduxjs/reselect) comes in handy when we compute derived state, which is pretty much done all the time.

![Redux-reselect-from-cache](https://productioncoder.com/wp-content/uploads/2018/10/redux-reselect-from-cache.jpg)

Given that we have two properties called *firstName* and *lastName* in our global state. Where in our component we want to display the users full name. So instead of pulling in *firstName* and *lastName* into pur component using a connect helper, we can just create a selector. The selector will be responsible in joining both the *firstName* and *lastName* with a space inbetween and will retturn a full name.

With the help of *Reselect* we can even cache the results, such that when the state is updated, our components perform a re-render. Suppose a different property in our state is updated, eg:- userHandle, but not firstName and lastName. Therefore, the value of the *fullName* will remain as same as before the state update.  Leaving the only thing that changes being the *userHandle*.

When the components re-render, they will call the selector and be expected to return *fullName*. Since nothing has changed the value will be fished from the cache and returned, reducing any addition work. fullName is supposed to be recalculated only if firstName and lastName is changed, and here Reselct will take care of this for us.

__Reselect only recalculates the values of the selctor if the part of the state that the selector is working with is updated.__

![redux_reselect_cache_recalculate](https://productioncoder.com/wp-content/uploads/2018/10/redux-reselect-cache-recalculate.jpg)

As you can see this is a great performance boost.

Reselect is one of the ways in which you can give a performance boost to your app, and infact make the code base much more extendable and maintainable.



