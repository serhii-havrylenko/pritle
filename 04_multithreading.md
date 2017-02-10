# General information

JavaScript has only one thread. There is no difference where JS code runs - in browsers engine or on the server side in NodeJS. JavaScript has event loop which operates with code execution order and manages concurrency.

The biggest feature of JS is that functions runs from the beginning till the end. It means that function cannot be stopped by some thread and resume it execution later (like thread1.pause() and thread2.resume()). In truth it can be done with async/await in ES7, but it is not officially released yet.

## Event loop

Here is the easiest visual representation of event loop:
![Event loop basic structure](https://mdn.mozillademos.org/files/4617/default.svg "Event loop basic structure")

Basically event loop works like:
* Event loop puts function to stack of frames (each function executes from stack of frames)
* Function executes, initializes all variables and runs till the next call of function
* When next function executes, event loop puts this function at the top of stack of frames
* When function returns, event loop pops element from the stack of frames
* If function calls system event (like I/O operations, timeouts, XHR requests, etc) event loop calls Web API (or system in case of Node). When event finished or triggered, Web API puts callback function to the queue (callback queue)
* When the stack of frames is empty, event loop checks callback queue and if there is something there it takes first element from it and puts it to the stack of frames and proceed with it like with standard function (executes it)

The best explanation(without words, only worked code with detailed visualization) how even loop works in JavaScript could be found [here ](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D) . It shows exact order of all events in real time with detailed info about call stack, web APIs and callback queue.


Here is another basic example of event loop and how web APIs + callback works:
```javascript
(function() {

  console.log('this is the start');

  setTimeout(function cb() {
    console.log('this is a msg from call back1');
  });

  console.log('this is just a message');

  setTimeout(function cb1() {
    console.log('this is a msg from call back2');
  }, 0);

  console.log('this is the end');

})();

// "this is the start"
// "this is just a message"
// "this is the end"
// "this is a msg from call back1"
// "this is a msg from call back2"
```

## Multithreading

Multithreading in JavaScript can be achieved using callbacks calls. In general it won't be a classical multithreading with ability to start/stop/pause/resume processes.

Another way of creating multithreading application in JavaScript is using HTML5 web workers for running code in background. Web workers runts in separate thread and helps with realization of classical multithreading in JavaScript. They have some limitations (like no access to the DOM, global variables, window, etc), However communication between web workers and other code which has an access those variables can be done through events.

![Web workers structure](http://blog.carbonfive.com/wp-content/uploads/2013/10/web-workers.png "web workers")
