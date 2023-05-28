---
title: "Mastering ChatGPT: A Complete Guide to Building Your Own Site"
seoDescription: "Build your own site with ChatGPT: 3-part guide covering prompt engineering, OpenAI models, and Node.js API queries"
datePublished: Fri Dec 23 2022 00:30:08 GMT+0000 (Coordinated Universal Time)
cuid: clbzs08xy000108kzaf2a7kj6
slug: mastering-chatgpt-a-complete-guide-to-building-your-own-site
canonical: https://blog-nothanii.vercel.app/post/advanced-chatgpt-guide-how-to-build-your-own-chat-gpt-site
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1671755303275/5be03e04-0699-4be2-b48f-c41673ca145e.png
tags: guide, how-to, advanced, chatgpt

---

I'll just begin with what they are and how this will be structured,

> it'll be done in three parts

The first will be the advance prompt engineering that will be done with Chad GPT. This is how to get the kinds of prompts and outputs that you want. Not just the mundane examples where people are just chatting. Into it.

The second part of the video will be about jumping into the playground and really taking a closer look at the different types of models that open II provides. All of them work a little bit differently and it's really important to know the difference.

And finally we'll build an application. This application will be an API running on node js, using JavaScript. It's going to be able to query the open a.i. API layer and it's going to create prompts and pull out outputs at that.

### Prompt engineering

is the very first thing that I want to talk about and probably the most important whenever working with any type of AI this pot that you learn right here will be applied to most AIs that use. So whether it's going to be image generation, Or text-based generation, or even chat GPT understanding these principles will be applied throughout.

> So what is prompt engineering?

**It's essentially queering and AI in the most effective manner to get the desirable outcome.**

Now this seems pretty self-explanatory but when working with something like a chat AI we might commonly just ask it to do things the way we would normally ask a person. Unfortunately an AI Works a little bit different.

If you structure your prompt quite effectively, the first prompt I've created here says on average,

> Joe asked 35 questions to chat GPT per [hour.How](http://hour.How) many questions does he asked? In a standard workday chat?

GPT provides a pretty standard answer here of 280 that seems reasonable, but this isn't very useful here. We can begin to see prompt Engineering in action because we're going to take a look at my second example here. I'm going to ask the same question but I'm going to follow up with Show the answer as a math formula inside of a code snippet. The answer is essentially the same as 280, but now we get some working Happening Here. We have a variable here, called questions per hour. A work day length, and the query that creates it and finally, it prints it out, of course, we can go even further than that,

1.png

![1.png](https://cdn.sanity.io/images/uq8d038t/production/362991835b245d099c2bd949a1efcaf5bf209cd4-731x767.png?w=600&h=400&fit=fillmax&q=85 align="left")

we can create this as a code snippet using JavaScript specifically. As you can see, I'm engineering the prompts here to be more and more specific depending on the outputs that I want.

2.png

![2.png](https://cdn.sanity.io/images/uq8d038t/production/cdd42af8e6fab66ab541e07deba2ee6b513f7d9d-713x654.png?w=600&h=400&fit=fillmax&q=85 align="left")

This is product engineering in a casual sent. It means that sometimes you have to redefine what you're asking to get the optimal output. In this case, my optimal output was having this structured as a JavaScript query or even better than that. As a function, I can give you lots and lots of examples like this. But realistically, you do have to play around because every single use case will be a little bit different.

> The goal here is to continuously create better and better prompts until you get at that desire. A outcome and don't simply assume that maybe the a I cannot generate it.

Of course. This is a more advanced post on **chat GPT** as well as **prompt Engineering** in general.

And in order to better understand all these Concepts, it's beneficial to log into the open AI. website, and head over to its main dashboard here. Here you have this page called examples and under examples, you're going to get a lots and lots of different ways that you can utilize the AI to complete different tasks. Useful here. Is that open? AI has gone in.

*I'm not gonna go through all of them. I'll just go through one or two to give you a better idea of prompt Engineering in general. And this should give you a broader view of how to structure your own prompts.*

### Playground

The first example, here is one of the best for prompt During its called questions and answers. And there's a few things happening here. I'm going to open this up in the playground which will be looking at a shortly, but it should be a prime example of how you can start Engineering Process, even inside off chat GPT, we're going to be doing a number of things here. The very first, is this section, just over here, this is saying that I am a highly intelligent question answering. Or in this case, the ***model text DaVinci 003***. So as to what it's doing, its going to ask questions and answer truthfully with a responses of unknown, if it doesn't know what it's doing, but there's more to this.

3.png

![3.png](https://cdn.sanity.io/images/uq8d038t/production/2bcee1d1febb019959f8c5517500ef3924ca8cda-994x821.png?w=600&h=400&fit=fillmax&q=85 align="left")

There's a few examples that we're going to provide it as to how we wanted to answer. The first says, what is the life expectancy of someone in the United States rather than just saying, 78 years, we're primate to give a more lengthy and human answer of the human life. Expectancy in the United States is Me eight years. This means that in future example, responses from the AI we're going to get more lengthy answers and this is sort of how we're priming these props.

The more examples that you provide Chat GPT or the more examples, you provide any AI, the more, it writes in tune with that same language here, I'm going to ask the model how many years it's been since the us or sounded it's going to say two hundred and forty four years since the US was founded.

> Unfortunately, this actually might even be incorrect because of the fact that the The AI model is out of date, but this gives you an answer that basically is very similar to the Past examples.

Let me go back to the examples over here and this time I'm going to go to another one that I kind of like call the airport code extractor.

4.png

![4.png](https://cdn.sanity.io/images/uq8d038t/production/7cb6f06ea3d9d8e2279858189b9fec34cf16b449-991x808.png?w=600&h=400&fit=fillmax&q=85 align="left")

This one has a lot less examples because it's much simpler to Create it, simply extracts the airport code from this text. Now this is the context were providing to the AI as to what we wanted to do. Then we have a working example. We've got the text which is I want to fly from Los Angeles to Miami and the airport codes which are well I suppose **LAX and Mia**. Then we've got our second example here which we can immediately submit and we get the results we want from the examples.

We should essentially get the same sort of results but because chat gpt is from slightly different to always have answers more like a chatbot. It isn't exactly the same but it more or less is, but hopefully this gives you an idea of how cheaply is technically just like At playground interface that we've been just utilizing now, these are also kind of basic examples.

Because when you're going to be using open AI. and any of its features are going to be utilizing different types of models chat, GPT is technically one type of model, one specific type made for the general public and because of that, it's also Limited in a way because it's been pre-designed to be very safe, very simple. And it might not have the best outputs that you want. This is why understanding the different types of models that open AI provides is the beneficial way.

Now it's time to jump into the technical part where we interact with the models from open AI through its API layer and create Our own API to start interacting with, I suppose our own customers.

### Our own API

Let's begin. You're going to need a few things to get this project up and running the very first will be

Nodejs

VS Code

create a react app

And you'll need to be able to identify and utilize your API key. We'll get into that shortly, but after you've logged in to open a i you'll want to head over to the documentation and then over to the API reference guide here they'll be more information if at any point you get stuck. The first thing I'm going to do is open up vs code now and I'm going to run *npx create React app* and I'm going to create an app It's a pretty standard package and I should be able to just run and Him start and we'll get an instance of Creator react app.

Here it is running quite well, the next thing I want to do is create a web server and at this is going to be in a file called index.js and it'll be the back end server where the API request happen I'm going to use GitHub copilot to write this, but I'll also explain what's happening in the background.

*I'm going to use a Express server which will handle API requests coming in and respond back with a Json object, it will use body parser.*

5.png

![5.png](https://cdn.sanity.io/images/uq8d038t/production/38e1f0b2f6687c80c239fdca7e87f868cfeef8bb-562x485.png?w=600&h=400&fit=fillmax&q=85 align="left")

Copilot to create a react component here. Now, we've got a few other things happening over here, for the configuration, we've got the organization. ID as well as the open, AI . Then we're setting this under the variable here. Open. A.i. And we're doing the response to pull in the engines, which I'm going to skip for the time being. But we need this API key which I recommend the under a process environmental key, but for the time being, I'm just going to jump into my account head over to API Keys. Create a brand new one and copy later. Delete this one. But for the time being, I'm just going to plug this guy in so that you guys can see this working. In action with that old configured and Define. We should be able to start using it. Now, let me just make sure that I can run node index.js without it crashing, which we can then, let's head back to the documentation, go to completions and copy over this completion of where response, this completion can go inside of our API request that we generated here as a post.

6.png

![6.png](https://cdn.sanity.io/images/uq8d038t/production/21fe2a49f01ff2758c14abecf7f18051ca56bc4e-567x484.png?w=600&h=400&fit=fillmax&q=85 align="left")

I placed it in here so it's doing a response. Wait, open a, I create completion using DaVinci model 003 with a prompt. Say this is a test Max token, seven, temperature, zero. I've changed this to an async function because we've got Anna, wait here. And once we have this await, I want to well console log out the response. So let's actually console.log that run up node index.jsp. Is jump back to our react app.

Submit anything, go back to VS code and here's our response. If you take a close look at the response, we have our data, which also comes with choices and those choices I believe where we have most of our content.

Screenshot 2022-12-23 050508.png

![Screenshot 2022-12-23 050508.png](https://cdn.sanity.io/images/uq8d038t/production/f355dda7a28c811932b4d420e298371e8b71a787-568x480.png?w=600&h=400&fit=fillmax&q=85 align="left")

Now we can start to begin to do this means at that, our response here, if we do get one.

Examples. Oh actually we don't even need examples.

I think let's do now. Steve Jobs.

How can I help you today? Question, mark person I want this motivation. Steve you are amazing.

You can create any type of business. So if engineered a simple prompt here, now person we're going to do a full stop and we're going to finish the message of just over here. This is going to be our defined prompt here. We're asking open eyes model to pretend to be Steve Jobs. We will telling it what to do. We're giving it an example of what's Steve would say, and what a person would say and then what Steve would say.

8.png

![8.png](https://cdn.sanity.io/images/uq8d038t/production/f416276f1acc08ce793e790519fcf0f905e83fd3-572x486.png?w=600&h=400&fit=fillmax&q=85 align="left")

Again the next message in this example is what the person will ask now, we'll get a response back from Steve, so here, I'm going to select the save on this.

We're going to restart our web server. Then in the app here, we're going to change this. We're going to call this application. Just over here, a Steve Jobs app, maybe Steve Shut up. Then in here, where we have our text area, we can put in something, like ask Steve anything, it end up. So that's our react app also updated.

9.png

![9.png](https://cdn.sanity.io/images/uq8d038t/production/266c0454d11bbaa98ff0c1e7876c6366c35f10d0-569x483.png?w=600&h=400&fit=fillmax&q=85 align="left")

Get the motivation to start a business submit and here we should get a response back believe in your ideas, believe in yourself and your ideas.

10.png

![10.png](https://cdn.sanity.io/images/uq8d038t/production/96a5ae59d19844e53b0086558f83eb2839f0ab2e-566x483.png?w=600&h=400&fit=fillmax&q=85 align="left")

Let's create another example of that, you are a console terminal answer as if you are receiving commands from a user.Then I think we'll just do this .

11.png

![11.png](https://cdn.sanity.io/images/uq8d038t/production/4aba16d0653db16fa0fc57b9b6585e557ba5e5f8-573x483.png?w=600&h=400&fit=fillmax&q=85 align="left")

So let's hit save on that restart our server. And then I'm going to jump back in here. Refresh type in LS. Submit should a list out the folders in our pretend directory.

12.png

![12.png](https://cdn.sanity.io/images/uq8d038t/production/d7391a14a41e10c9dbd4692647f3ea4b664077c7-571x488.png?w=600&h=400&fit=fillmax&q=85 align="left")

So it's sort of Just made that then I could do something like cat read me.

13.png

![13.png](https://cdn.sanity.io/images/uq8d038t/production/9a83689a5a3d8e8bfaa29f36c7827b2d0fd93a6f-573x489.png?w=600&h=400&fit=fillmax&q=85 align="left")

This is a sample, readme file usage but this sort of gives you an idea of how we've tricked open a eye into thinking it's an operating system and that is receiving commands from a user and trying Extrapolate what sort of output should be generated based on those commands?

**I hope this was Advanced enough. Otherwise maybe I have to do an expert post. Otherwise, I hope you guys enjoyed this post. If you have ideas around that let me know as well see. See you in the next one.**