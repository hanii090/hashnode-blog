---
title: "JavaScript Performance Tips"
datePublished: Thu Sep 22 2022 23:27:36 GMT+0000 (Coordinated Universal Time)
cuid: cl8dopayd03m4vnnv3txsb7o9
slug: javascript-performance-tips
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1663889762809/0Us2fj92j.png
tags: javascript, performance, tips, javascript-framework, javascript-library

---

 
**JavaScript**, like any language, requires us to be judicious in the use of certain language features. Overuse of some
*features* can decrease performance, while some techniques can be used to increase performance.
## Tip 1
- Avoid try/catch in performance-critical
functions

Some *JavaScript *engines (for example, the current version of Node.js and older versions of Chrome before
Ignition+turbofan) don't run the optimizer on functions that contain a *try/catch* block.
If you need to handle exceptions in performance-critical code, it can be faster in some cases to keep the try/catch in
a separate function.


```
function myPerformanceCriticalFunction() {
 try {
 // do complex calculations here
 } catch (e) {
 console.log(e);
 }
}
``` 

## Tip 2
- Limit DOM Updates

A common mistake seen in JavaScript when run in a browser environment is updating the DOM more often than
necessary.
The issue here is that every update in the DOM interface causes the browser to re-render the screen. If an update changes the layout of an element in the page, the entire page layout needs to be re-computed, and this is very
performance-heavy even in the simplest of cases. The process of re-drawing a page is known as reflow and can cause a browser to *run slowly* or even become **unresponsive**.


Consider the following document containing a **<ul>** element:
```
<!DOCTYPE html>
<html>
 <body>
 <ul id="list"></ul>
 </body>
</html>
```
We add 5000 items to the list looping 5000 times (you can try this with a larger number on a powerful computer to
increase the effect).

```
var list = document.getElementById("list");
for(var i = 1; i <= 5000; i++) {
 list.innerHTML += `<li>item ${i}</li>`; // update 5000 times
```





## Tip 3
- Benchmarking your code - measuring execution
time

Most performance tips are very dependent of the current state of JS engines and are expected to be only relevant
at a given time. The fundamental law of performance optimization is that you must first measure before trying to
optimize, and measure again after a presumed optimization.
To measure code execution time, you can use different time measurement tools like:


> 1. Performance interface that represents timing-related performance information for the given page (only available in
browsers).


 
```
let startTime, endTime;
function myFunction() {
 //Slow code you want to measure
}
//Get the start time
startTime = performance.now();
//Call the time-consuming function
myFunction();
//Get the end time
endTime = performance.now();
//The difference is how many milliseconds it took to call myFunction()
console.debug('Elapsed time:', (endTime - startTime));
The result in console will look something like this:
Elapsed time: 0.10000000009313226

``` 


> 1. process.hrtime on Node.js gives you timing information as [seconds, nanoseconds] tuples. Called without argument
it returns an arbitrary time but called with a previously returned value as argument it returns the difference
between the two executions.
> 1. Consoletimers console.time("labelName") starts a timer you can use to track how long an operation takes. You
give each timer a unique label name, and may have up to 10,000 timers running on a given page. When you call
console.timeEnd("labelName") with the same name, the browser will finish the timer for given name and output
the time in milliseconds, that elapsed since the timer was started. The strings passed to time() and timeEnd() must
match otherwise the timer will not finish.
> 1. Date.now function Date.now() returns current Timestamp in milliseconds, which is a Number representation of
time since 1 January 1970 00:00:00 UTC until now. The method now() is a static method of Date, therefore you
always use it as Date.now().


```
let t0 = Date.now(); //stores current Timestamp in milliseconds since 1 January 1970 00:00:00 UTC
let arr = []; //store empty array
for (let i = 0; i < 1000000; i++) { //1 million iterations
 arr.push(i); //push current i value
}
console.log(Date.now() - t0); //print elapsed time between stored t0 and now

``` 




## Tip 4
-  Initializing object properties with null

 
The best way to make an object predictable is to define a whole structure in a constructor. So if you're going to add some extra properties after object creation, define them in a constructor with null. This will help the optimizer to predict object behavior for its whole life cycle. However, all compilers have different optimizers and the
performance increase can be different, but overall it's good practice to define all properties in a constructor, even
when their value is not yet known.


 **That's it for Today**
  












