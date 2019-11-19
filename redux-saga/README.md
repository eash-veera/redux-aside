# Introduction to Redux Saga

Redux saga is a Redux middleware that allows you to manage your side effects (i.e. reaching out to the network) in an easire more testable way.

This aside is suppose to give a conceptual Redux-saga and explains its purpose.

# How does it work ?

The [redux-saga docs](https://github.com/redux-saga/redux-saga) states that the *'the mental model is that a saga is like a sepereate thread in your application that is solely resposnible for side effects.'* What does this mean ? Lets try to visualise this.

For the visualization purposes lets assume that we have a button in our application thats loads some data when clicked. Simplicity sake, lets assume we only have one action called FETCH_DATA. Note that the illustration is not perfectly accurate. Technicaly speaking the redux middleware is part of our top level /App component. How ever for illustrations purposes, the imges sort of suggest that the redux-sage middleware and the app components are somehwhat seperated. 

We do this because redux-sage is easier to understand with a few illustrations.

![redux_saga_on_click_create_action](https://productioncoder.com/wp-content/uploads/2018/10/redux-saga-on-click-create-action.jpg)

In the above illustration circles are supposed to represent actions, i.e. Javascript objects with a type property. The redux-saga middleware listens to all actions that are dispatched in our app. A saga listening for a specific action type. In the illustration, the red circle represents the FETCH_DATA action.

# Consuming actions

As soon as an action is dispatched the our saga is listening to (the red circle), our saga will consume it. You can think of this like taking away the action from the overall stream of actions.

![redux-saga-consume-action](https://productioncoder.com/wp-content/uploads/2018/10/redux-saga-consume-action.jpg)


# Watcher sagas and worker sagas

![redux_saga_fork_worker_saga](https://productioncoder.com/wp-content/uploads/2018/10/redux-saga-fork-worker-saga.jpg)

Once our action is inside our saga, our sagas will probably extract the payload from it. The payload typicaly contains the parameters we need for our API calls in our actions. After extracting the payload, the co called watcher (left) typiclay creates / forks a worker saga that will do the actual request.

When using redux-saga, you typiclay end up with two sagas for a particular action, watcher saga (left) workr saga (right). 

* The watcher saga is quite primitive, in that it watches for action's with a specific type e.g. FETCH_DATA is dispatched and extracts the payload if needed. If the watcher saga detects the action its looking for, it then creates a worker saga.

    * The watcher saga can either wait for the results from the worker saga, or resume listning for new actions or it could perform a fire and foreget appraoch, by forking a worker sagaand immediatley returning to listen for new actions. Both approaches make sense depending on what you aim to achive.

* Once the woker saga gets results back (either success or failure ), it is dispatching an action with server's response (F). This action then flows into the reducer where the next state is calculated. In the illustration, its assumed that the response was successful.

__For a much more comprehensive understanding of how redux-saga works refer to the original [documentation](https://redux-saga.js.org/docs/introduction/BeginnerTutorial.html)__

