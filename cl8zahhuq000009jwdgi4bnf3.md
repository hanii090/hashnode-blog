---
title: "Flexbox Fundamentals"
datePublished: Sat Oct 08 2022 02:20:33 GMT+0000 (Coordinated Universal Time)
cuid: cl8zahhuq000009jwdgi4bnf3
slug: flexbox-fundamentals
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1665696205954/AYzoRv2ns.png
tags: web-development, webdev, beginners, web3, beginner-developers

---

 
## Using flex-direction to layout content horizontally and vertically
 
Flexbox is a layout mode in CSS. This layout type makes it easy for developers to display content in various ways.

Since browsers display content vertically – *from top to bottom* – by default, the Flexbox layout mode gives devs more "flexibility" with how their content is displayed on the front-end.

### Well, we know what CSS is, *but, what's a "layout mode?"* Here's a refresher:

Layout modes – *or simply "layouts"* – are sets of instructions that tell the browser the position and size of boxes based on the code's hierarchy. 

While there are several types of layouts, this lesson focuses solely on *Flexbox* – a layout mode that makes building responsive sites easy and looks cool!
 

## Defining a Flexbox container

In the code example, instructor Garth Braithwaite is defining a Flexbox container by turning an ordered list into a navigation bar.

First, he removes the bullets from the `ol` by setting `list-style:;` to `none`.

Then he sets the `display:;` to  `flex`. Whatever code is written after this will be treated as "children" and styled accordingly.

### Note: Flexbox containers have a property called `flex-direction`.

`flex-direction` defaults to row which is displayed horizontally.

Following the transcript, we can note that this flexbox property's value can be changed to:

- `row` – This is the default position for the `flex-direction` property.
- `row-reverse` – The children will flow from left to right.
- `column`– The column will display vertically.
- `column-reverse` - The children to flow from bottom to top.

---

The resulting code snippet for a horizontal navigation bar in Braithwaite's example is:

```
.main-nav ul {
    list-style: none;
    display: flex;
    flex-direction: row-reverse;
}
```

## Using order to rearrange flexbox children
 

What's great about flexbox containers is their ability to be rearranged with the ``order`` property. You can place your elements wherever you'd like with this property.

In Garth's example, the main navigation bar will be placed below the footer to "free up some real estate" or give the most important content on the page priority. This rearrangement will also make mobile responsiveness look much less bulky.

Since ``order`` defaults to zero, your children will flow in the order in which they appear in the Document Object Model, or **DOM**. Learn more about the DOM [here](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction).

The example shows the four primary sections (containers) on the web page that will be rearranged:

- Header – 0 (default order)

- Navigation – 0 (default order)

- Content – 0 (default order)

- Footer – 0 (default order)

In the example, Garth wants the navigation **just above** the footer. To do this, the body will need to display the containers as `flex` so that we can rearrange them.

**Nifty trick**: Since we want the containers to flow vertically, think of your mobile phone and how you'd like to see the containers displayed 📲. *How would this be written in your code?* Like this:

`flex-direction: column;`

The only thing left to do is rearrange your containers by reordering their placement on the webpage. To do this, simply change the order property on the individual children!

In the previous example–where all the children are defaulting to zero–the order properties need to be updated in order for the nav container to appear just above the footer. The order would be:

- Header – 0, where it would remain at the top of your webpage.

- Navigation – 1, same order as Footer BUT it's new placement in the DOM is written below the `body` styling and will show up as ordered (the last container on the webpage).

- Content – 0, where it would be shown underneath the Header.

- Footer – 1, while it appears to be the same order as Navigation, the Footer's placement in the DOM *in relation to Header and Content* will determine where it shows up in the browser. Note that it is written above the `body` and its `order` is also in relation to the the Navigation's placement in the DOM.



## Demystifying alignment in flexbox children
 

Because flexbox layout is so * *ahem* * flexible, it can be difficult to remember which properties do what and what values to use when you need an element to be positioned just right in the browser.

In this lesson, the following properties are detailed:

- `justify-content`
- `align-items`
- `align-self`

The values for each of these properties will be detailed, as well.

---

### Before we jump in, let's get a quick refresher on `flex-direction` 👍:

`flex-direction` defaults to row which is displayed horizontally.

Following the transcript, we can note that this flexbox property's value can be changed to:

- `row` – This is the default position for the `flex-direction` property.
- `row-reverse` – The children will flow from left to right.
- `column`– The column will display vertically.
- `column-reverse` - The children to flow from bottom to top.

---

## Alignment via flexbox children properties

### `justify-content` affects the way the children are aligned along the direction the content is flowing.

In other words, depending on where your element starts, the justify-content will affect how the extra space along the flex directioin is used.

Think about it: Is your flex-direction set to `row` (that's horizontally)? Then, `justify-content` will affect the extra space at the top, or start, of your element... but, *why?*

It's because `justify-content` defaults to `flex-start` meaning that the child will crop up the its starting point. In the case of Garth's example, the children are all starting at the top.

#### `justify-content` values include:
-   `flex-start`    - the child will start at the container's top
-   `flex-end`  - the child will start at the container's bottom
-   `center`    - the child will start at the center, or in the middle, of the container
-   `space-between` - if there are multiple child elements, this value will stick one child to the start of the flow and the last child to the end, with the children in between spaced out evenly.
-   `space-around`  - the space around each child will be evenly distributed.



---

### `align-items` declares how to use the space perpendicular to the `flex-direction`

`align-items` defaults to stretch. Because of this, your browser will show the child element at 100% of its container height.

#### To move items within their containers, you can use `align-items` values which include:

-   `flex-start`    - this will move your children to the top
-   `flex-end`  - this will move your children to the bottom
-   `flex-center`   - this will move your children to the center
-   `flex-baseline` - this will move your children to the baseline of the first line of text in the element.

---

### `align-self` is the same as `align-item` but is applied to specific, individual children

`align-self` defaults to `auto`, meaning that it will do whatever `align-item` tells it to do.

So if you want a specific child to display in the container in a way that's different from the other children, you'll have to give it a value to `align-self`.



## Defining Dimensions on Flexbox Children Using Flex-Basis
 

Depending on your needs, Flexbox can be coded to do many clever things. Starting with sizing, we'll learn about the following in this lesson:

-   The `flex-basis` property and its values
-   The importance of cascading hierarchy
-   What the `flex-direction` axis is
-   Why width and height aren't always the best sizing properties to use for Flexbox

---

### 3 Properties That Size Flexbox Children:

Following the transcription notes, the following three properties handle the resizing of flexbox children (not the flex-container) along the `flex-direction`:

- `flex-grow`
- `flex-shrink`
- `flex-basis` 

---

## `flex-basis`

`flex-basis` is used to define the optimal size of the child element, along the `flex-direction`.

**NOTE**: The parent container in this example is the `body` tag. When Garth gives the parent a display property value of `flex`, the children follow the rule.
So instead of defaulting to their box container sizing (that's 100%), the children take on the size of the content within their respective containers. Why? Because these *well-behaved children* follow the values of their parent.

### The importance of Cascading Heirarchy

The children are taking on the width of the parent element in the example. But what if the children want to take on their own sizes? The best way to do this for flexbox children is to ditch the standard `width` and `height` properties and assign them `flex-basis` properties and values.

`flex-basis` defaults to `auto`, making the child follow the dimention set by the `flex-direction` (e.g., a row, a column, etc.).

**NOTE**: Flex-basis is the ideal size for the element along the flex-direction if it has enough room. - Garth B.



## Using `flex-shrink` and `flex-grow` to Make Flexox Children Resize Correctly
 

What are we to do with excess space, or lack of space, around our flexbox? We will want to use either `flex-shrink` or `flex-grow`.

What's really neat about either property is that they directly impact how your flexbox children will populate on your web page.

Following Garth's example, you'll see that the `body` is still set to `display: flex;` and each child has their own `flex-basis` values. This is great because now we can work with the space within the container, and with Garth's dimensions, there's a lot of space to work with.

# Distribution of Extra or Unused Space in the Flexbox

## `flex-grow`

To use, or fill up, the extra space in the flexbox we want to use the `flex-grow` property. It tells the browser precisely how much space you want each of the children to occupy within the container.

**NOTE**: `flex-grow` defaults to a proportion of zero; none of the children will grow past their `flex-basis`.

If you want the first child element to take up all the space, set its proportion to one.

What about the second and third children? Set their flex-grow proportions to the values of your choice.

In Garth's example, he set the second child to two (using up 2/3 of the container space; the first child will use up 1/3). He then set the third children to three, which adjusted the amount of container space that *all the children* used up. The result:

-   The first child: 1/6 of the container's extra space 
-   The second child: 1/3 of the container's extra space
-   The third child: 1/2 of the container's extra space
 
### Distribution of Total Space in Flexbox

Setting the flex-basis of all children to zero will prepare us to use all the total space on our container. That means the extra space wouldn't be taken up by any of the children, making it the same as the container's total space.

**Note**: `flex-grow` dictates how the extra space beyond the combined `flex-basis` should be divided up.

## `flex-shrink`

### Note that the objective of `flex-shrink` is that "content should never be clipped for the benefit of empty space."

This property works like `flex-grow`, but in reverse.

To illustrate how `flex-shrink` works, Garth sets the children's `flex-basis` values to 200 pixels, totaling 600 pixels in width; exceeding the width of the container. Each child is too big for the container – 😱 Yikes! They're even losing some width because they cannot fit into the container.

But... *there's space left over from the container causing the children to not fit*... Why? Well, if the combined `flex-basis` is greater than the container's allowed space, the child elements will shrink to fit!

You'll see that these elements will loss some width since `flex-shrink`'s default is one and is corresponding to the `flex-basis` value.

If you don't want the children to shrink, set each one of them to zero. This will cause them all to shrink in correspondence to the `flex-basis` value **and** cause the container width to overflow.




## Combining the Flexbox Sizing Properties Using the Flex Shorthand

 

Flex shorthand is super cool but might cause some weird things to happen in your browser.  *Using `flex-shrink` and `flex-grow` to Make Flexbox Children Resize Correctly*.

### NOTE: Flex shorthand makes flexbox sizing consistent, but watch out for the defaults!

---

## Flex Shorthand – What Is It Telling The Browser?

### The `flex` property is telling our browsers three distinct things: *How to size our container's three values of `flex-grow`, `flex-shrink`, and `flex-basis`*.

NOTE: When using flex shorthand **consider the defaults** of your values. Garth explains further in the lesson.

Although this is mentioned at the end of the tutorial, let's start thinking about the flex property with a value of one, or `flex: 1;`.

In the example, we're telling the browser to let Heading 1 display with a `flex-grow` of one, a `flex-shrink` of one, and a `flex-basis` of zero. As a result, you're going to see that the this element will have children that grow to use all the container space evenly.

See below and in minute [01:27](https://egghead.io/lessons/flexbox-combining-the-flexbox-sizing-properties-using-the-flex-shorthand#t=84) of the tutorial:

```
h1 {
    flex: 1;
}
```

Clean enough, but what this shorthand code (above) is actually saying to the browser is in the commented out code. Take a look:

```
h1 {
    flex: 1;
    /*  flex-grow: 1;
        flex-shrink: 1;
        flex-basis: 0px;
    */
}

```

Now, let's take a look at the shorthand examples Garth provides in the tutorial. The commented out code has been removed.

### Example 1
````
.title-1 {
    background: #dd5f40;
    flex: 1;
}
````
What is this code telling the browser:

The `flex` has a value of `flex-grow: 1;`, `flex-shrink: 1;`, and `flex-basis: 0;`. Normally, your flex-basis would default to `auto`.

### Example 2
````
.title-2 {
    background: #3d483a;
    flex: 20px;
}
````
What is this code telling the browser:

The `flex` has a value of `flex-grow: 1;`, `flex-shrink: 1;`, and `flex-basis: 20px;`. The pixel is a unit of measure, making it appropriate to pass into the flex-basis.

### Example 3
````
.title-3 {
    background: #468e5d;
    flex: 0 80px;
}
````
What is this code telling the browser:

The `flex` has a value of `flex-grow: 0;`, `flex-shrink: 1;`, and `flex-basis: 80px;`. Your flex-grow is a "unit-less" measurement of zero, and your flex-basis is a unit of measure at 80 px.


## Turning a Flexbox into a Grid Using flex-wrap and align-content
 

Ahhhh... The moment we've been waiting for: Turning `flexbox` into a grid. Following Garth's instrustional notes: Adding flex-wrap to a flexbox container allows the items to form a grid. The content can then be aligned and distributed along the grid using justify-content and align-content.

---

## How to Make a Flexbox Grid

Starting with the code example, update your `display` to flex. Be sure to nix the `width` property as you won't need to set it thanks to `display: flex;` which gives your browser your container's width sizing information. And keep your `flex-grow` value at zero–this will stabilize the container's display behavior in the browser.

```
body {
    display: flex;
}
```

Now, you'll want to determine how you'd like your content to be displayed. Garth recommends keeping all your items on the page (in the example, the images are running over the page parameters). In his example, the `flex-wrap` is set.

Although `flex-wrap` defaults to `nowrap`, it has several values such as `wrap` and `wrap-reverse` which causes the line breaks to flow up instead of down (might vary with your code).

```
body {
    display: flex;
    flex-wrap: wrap;
}

```

Now, if you're interested in spacing the containers' contents, use the `justify-content` property which has a host of values for you to choose from. Learn more about the values [here](https://www.w3schools.com/csSref/css3_pr_justify-content.asp).

In  example, we're going to us `space-around` which will give us space along the flex direction axis.

````
body {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
}
````

Next, we want to **consider how the content is aligned**. In the example, we're concerned with the *vertical alignment* of our content.

Alignment values vary, but include ones we've been working with throught the Flexbox Fundamentals instructions:

- `flex-start`
- `flex-end`
- `center`
- `space-between`
- `space-around`
- `stretch`

Keep in mind that it is *only the content* that is being aligned, not the container itself.

Final example from Garth's instructions:

````
body {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    align-content: space-around;
}
````






🤔 Learn more about layouts **[here](https://developer.mozilla.org/en-US/docs/Web/CSS/Layout_mode)**.

🤔 Resources for alignment: [align-items](https://developer.mozilla.org/en-US/docs/Web/CSS/align-items), [align-self property](https://www.w3schools.com/CSSref/css3_pr_align-self.asp)

🤔 Learn more about justify-content values, including `initial` and `inherit` here: [MDN web docs – justify-content](https://developer.mozilla.org/en-US/docs/Web/CSS/justify-content)


















