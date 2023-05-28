---
title: "CICD Requirements , YAML intro, Objects  In YAML"
datePublished: Fri Nov 04 2022 03:46:06 GMT+0000 (Coordinated Universal Time)
cuid: cla1yfiih000i08mj8pg7bt99
slug: cicd-requirements-yaml-intro-objects-in-yaml
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1667533364097/zSS05YQbj.png
tags: yaml, ci-cd, github-actions-1

---

 ## Intro


**Hello**, everyone. 

Welcome back  in this brand-new POST will be talking a bit about GitHub actions, continuous integration and continuous deployment. 

> Why that is important?

And 

> how you can set up these basic pipelines for your project on *GitHub actions*.

 Now, by the end of this course, you would not just be able to understand what** GitHub action** says and what  *CICD * is. 

But also would have built your few small build **pipelines ** where your project is being built tested and then deployed automatically where ever you want. 
 


## why is CICD required


![a.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1667530810014/IQyLMfEhV.PNG align="left")

All right, 

in this course, I'll be following up with a simply react playground.


![tempsnip.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667531173320/hpYDqWqg6.png align="left")

 I'm just going to say CICD with GitHub as the name and you can pretty much follow along on your local system.

 And once you get started with the project what we'll be doing is not focus on the project that much but focusing on something known as YAML files in this little series and get of actions. Eventually, let's talk a little bit about **GitHub ** actions as well. Well, so did GitHub  actions is basically a service, which allows you to do, 



> continuous integration, and continuous deployment.

Now, let's actually understand 


- what this exactly is.And why do we need this? 

These kind of things when you see when you're building a project like you know, one on your local systems, what you are essentially doing is working in the best possible and the best comfortable way you can as a developer, right? 

So you are working on all of the code and folders and everything as as a nice experience for yourself. But a lot of times, your project actually needs a build step and sometimes even a compilation step could become production-ready. That means, trimming off on the quad code, compressing the code, making the code more performant. And so on, and all of this build step in compilation. Step takes time to run. So what these *CICD *services do is in a, in a,  nutshell, basically they allow 

> what you have coded or written, or any member of your team has coded or it'll build and Deploy on staging production or preview environment, whatever environment you have automatically. Once you push on a repository like GitHub.



 Now, this is again important because you don't want as a developer. You don't want to spend a lot of time, just building and testing everything yourself on your local system. 

> Why?


Because it wastes your time and wastes the capacity of computing, you have on your system.


So you want to make sure that your deployment and testing part is as automated as possible. So that you as a developer spend, most of the time working on logic and building new features instead of actually building the project itself and testing it yourself GitHub provides this as one of the services. But GitHub is not the only place where you can run CICD. There are basically countless tools out there, which can do that, but we prefer GitHub. I prefer GitHub in this one because 

>  GitHub actions actually sits across your source code. Right? So when we will take a look at how we push this project together and so on I'll actually show you that basically like you know GitHub as just very convenient to push your source code to.

## Yaml Introduction



![Capture.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1667531985694/XldoiEZ-z.PNG align="left")

 I want to talk a bit about YAML.

     > what this exactly is

 YAML is *not a markup language* or sometimes some people say yet, another markup language as well. But the point you have to remember, is 

> YAML is pretty much what JSON is, right?

So you have seen stuff like package.json right? When you take a look inside package.json, you see there are these objects which can contain keys and values keys and values, right? Yeah, YAML is pretty much the same thing. The only difference is how YAML has written Json is used as a thing to communicate between different kind of languages and build systems and, you know, environments, right? So package.json can be read by JavaScript as well. Package.json this file, technically could also be read by a Java program or a python. Program, right? So let's say, if your Nodejs script wanted to spit out some data, which your python Market would read then. One way to do that, is to spit out that data into a Json format. Similarly, YAML is also a similar format for which you would have, you know, you would require a few libraries. For example, there is no native support for able in node.js, but it's more like a data exchange format among different you know, different languages. Different systems. Now, what YAML does better over Json is readability? Right, so we're gonna see eventually like this is, this is also quite readable, but YAML is even more cleaner. Why? Because it uses very less special characters. Like usually, there is no need of these codes. These brackets, these commas. Nothing is required by. There is very very minimal language syntax, which you need are a lot of information which needs to be go, you know, needs. To go in the file is just basically what your logic is for the data. So this little section or in this collection we're going to learn about YAML a bit, because it is really important to understand. YAML first, before you actually start writing work profiles because this is almost like a new kind of data format, which you are seeing right.  Let me go ahead and get on with you who the  YAML syntax a bit from the next one. 

 
![y.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1667532640295/wwcvb0h_Y.PNG align="left")



##  Objects In YAML


Let's start with YAML Basics,


So we're going to cover a few important things in the YAML . The first thing is 

> how you generate keys and value beard

 So in YAML what you do is you say key name and there are Colin and then the value, whatever that value is right. So this is like a completely valid YAML file. Similarly, let's say if I have a service My project that I could have, you know, tests or script as script one, then test to as script 2 and so on. So what essentially we are doing here is if you want to compare it with a JavaScript based, rendering something equivalent in JavaScript  YAML has much more condensed information compared to JSON

Just gets more condensed as we Nest objects and I'm as we go ahead and start working with that. So you can see right now that we just created some top-level keys and value pairs. Now it is possible in YAML to also go ahead and create nested objects, right? And the way you do that is pretty much with indentation so you can see my app as the key of A, you know, a parent object.


 And for these three keys, you have I really need them now. Very important YAML ,

 > just like in Python that these indentations are actually consistent with each other, right? Whether they are spaces or tabs doesn't matter, but they have to be consistent with each other. That if you are using one thing, then it has to be one thing all across your file. A similar thing. 


![t.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1667532690923/P1JzRKVYt.PNG align="left")

The similar logic in Json would look something like this in my app and then an object, something like this, and yep. There we go. So these two files over here at technically equivalent 

In terms of the data, they contain, right? So this is important because now you can see that it's much cleaner and much more human-readable compared to JSON.  


### ***TO BE CONTINUED  ***