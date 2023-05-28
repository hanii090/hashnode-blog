---
title: "Send Notifications Using  Pure Javascript"
seoTitle: "Javascript  Notification 🔔"
datePublished: Thu Sep 22 2022 06:24:37 GMT+0000 (Coordinated Universal Time)
cuid: cl8co5qvc004jwhnvgsoqdmtj
slug: send-notifications-using-pure-javascript
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1663827772132/LFS_WvE0X.png
tags: javascript, javascript-framework, javascript-library, javascript-modules, push-notifications

---

##  Requesting Permission to send notifications

We use **Notification.requestPermission** to ask the user if he/she wants to receive notifications from our website.


```

Notification.requestPermission(function() {
 if (Notification.permission === 'granted') {
 // user approved.
 // use of new Notification(...) syntax will now be successful
 } else if (Notification.permission === 'denied') {
 // user denied.
 } else { // Notification.permission === 'default'
 // user didn’t make a decision.
 // You can’t send notifications until they grant permission.
 }
});


``` 
Since Firefox 47 The *.requestPermission* method can also return a promise when handling the user's decision for granting permission

 
```
Notification.requestPermission().then(function(permission) {
 if (!('permission' in Notification)) {
 Notification.permission = permission;
 }
 // you got permission !
 }, function(rejection) {
 // handle rejection here.
 }
);
```
 

## Sending Notifications


After the user has approved a request for permission to send notifications, we can send a simple notification that
says *Hey *to the user:


```
new Notification('Hey', { body: 'Hello, world!', icon: 'url to an .ico image' });

``` 
This will send a notification like this:

> Hey
> Hello, world!

## Closing a notification

You can close a notification by using the *.close()* method.




```
let notification = new Notification(title, options);
// do some work, then close the notification
notification.close()

``` 
You can utilize the setTimeout function to auto-close the notification sometime in the future.

```
let notification = new Notification(title, options);
setTimeout(() => {
 notification.close()
}, 5000);

``` 
The above code will spawn a notification and close it after 5 seconds

## Notification events

The Notification API specifications support 2 events that can be fired by a Notification.

1. The click event.

This event will run when you click on the notification body (excluding the closing X and the Notifications
configuration button).

Example:
```
notification.onclick = function(event) {
 console.debug("you click me and this is my event object: ", event);
}
```
2. The error event

The notification will fire this event whenever something wrong will happen, like being unable to display

```
notification.onerror = function(event) {
 console.debug("There was an error: ", event);
}
```





















