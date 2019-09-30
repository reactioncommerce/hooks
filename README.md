> ## ⚠ **Warning**: This project has been archived and is not maintained.



# Hooks

Reaction Hooks provides a simple API that allows you to attach arbitrary callbacks to any particular event that you define. You simply
decide what your events are and then add callbacks to it from anywhere in the app. Then you can run all callbacks when that event
is fired. The canonical examples are `onCoreInit`/`afterCoreInit` event hooks that run code either during Reaction's
startup sequence, or after it has completed. (the on/after is just a naming convention, but we suggest that code extending
this code utilize this same convention)

## Install

```js
import Hooks from "@reactioncommerce/hooks";
```

## Usage

The API has the following methods:

`Hooks.Event.add(name, callback)`

Here you pass in the name of the event as a `String` and then the callback function you wish to be executed.

`Hooks.Event.remove(name, callback)`

Remove a named callback function that has already been added to an event.

`Hooks.Events.run(name, item, constant)`

For already defined events you will never need to use this, but if you are defining your own events this is what you
would execute to fire the event hook. The name argument is the name of the event, the object/modifier on which to run the callbacks,
and a constant to be passed to every callback (e.g. for a user-related event, it might be the user)

## Example

Let's say we wanted to create our own event for "onCreateUser". We would call the `run` method when the event occurred.

```js
import Hooks from "@reactioncommerce/hooks";

Accounts.onCreateUser(function (options, user) {
  // add a hook to alter the user object or do something with its data
  user = Hooks.Events.run("onCreateUser", user, options);
  // return the mutated user object
  return user;
});
```

Now you can pass any amount of functions into that hook from anywhere else in the app

```js
import Hooks from "@reactioncommerce/hooks";

// create a callback to run
function logUserEmail(user) {
  console.log("User being created with email: " + user.emails[0].address);
  // do whatever with the user doc and then return it
  // to the next callback
  return user;
}

// add your callback to the hooks array created above
Hooks.Events.add("onCreateUser", logUserEmail);
```
