---
layout: post
title: Partial Application with JavaScript
---

If you haven't heard [functional programming](http://en.wikipedia.org/wiki/Functional_programming) is all the rage these days. One of the more interesting aspects of functional programming is something called [partial application](http://en.wikipedia.org/wiki/Partial_application) which essentially comes down to preloading arguments in a function.

But George, if I'm going to pre-load functions with variables why don't I just use regular instance variables instead. Well, that's silly and a good way to write more boilerplate or even duplicate code. When you're using a language that supports [higher order functions](http://en.wikipedia.org/wiki/Higher-order_function) you should really take advantage of partial application (or [currying](http://stackoverflow.com/questions/36314/what-is-currying)) to make your code more polymorphic. Essentially it gives you more value from your function. Enough gibber gabber, let's write some code!

```javascript
function sendMessageTo(name, message) {
  console.log(name + ', ' + message);
}

sendMessageTo('George', 'clean your room!'); //George, clean your room!
```

So what I've just done here in `mom.js` is created a function that addresses a console.log to a name with a message. Super complex stuff. I then had it tell me to clean my room (over my dead body mom). But say, what if we wanted to tell me more than one thing. Like do the dishes, mow the lawn, perhaps even a bit of laundry?

```javascript
sendMessageTo('George', 'do the dishes'); //George, do the dishes
sendMessageTo('George', 'mow the lawn');  //George, mow the lawn
sendMessageTo('George', 'do the laundry');//George, do the laundry
```

Wow, she's furious and there's no way I'm doing all of that. It's unfortunate she had to specifically say to tell George that each time too. Recalling your own son's name is old school, computers should retain that info for us. Here comes partial application to the rescue!

```javascript
var sendMessageToGeorge = sendMessageTo.bind(null, 'George');

sendMessageToGeorge('be nicer to your mother'); //George, be nicer to your mother
```

Hey, that's neat. Now she doesn't have to pass in a name each time. It's almost like she's got a direct line! Let's break down what happened here.

JavaScript functions have a method called [bind](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Function/bind) (at least [es5](http://en.wikipedia.org/wiki/ECMAScript#ECMAScript.2C_5th_Edition) asks that most implementations do) which does 2 fancy things. The first argument I passed in was `null`. All this does is set the `this` context, a much bigger concept than the one I'm trying to convey right now. Essentially it allows you to change what `this` means **inside** the function. I passed in null because I don't care about that in this instance, much like the laundry. 

The next argument I passed in was the *first* argument of the function itself. It doesn't have to stop at 1 either, you can pass in all of the variables for the function if you would like.

```javascript
var sayHiToGeorge = sendMessageTo.bind(null, 'George', 'hi');
sayHiToGeorge(); //George, hi
```

That's rather pointless but you can do it. You can pass in more too but they won't go anywhere (unless you are grabbing them from the `arguments` hash).

So there you have it, partial application in JavaScript. It's a neat thing and can be very useful. Functions allow us to break our code up into nice concise blocks of functionality and partial application gives us the power to customize those functions as we need to without repeating code. Next time I'll touch on how binding `this` to the function can benefit you as well. Until then, happy JavaScripting!
