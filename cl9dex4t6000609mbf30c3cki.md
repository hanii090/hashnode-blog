---
title: "How to Build a Map with React"
datePublished: Mon Oct 17 2022 23:33:27 GMT+0000 (Coordinated Universal Time)
cuid: cl9dex4t6000609mbf30c3cki
slug: how-to-build-a-map-with-react
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1666048616465/IjrADRe1_.png
tags: javascript, reactjs, beginners

---

**Objectives 🧐**

we'll be building a Restaurant Locator using **Leaflet **to display the different locations on a map.

How?

- We'll be using a `create-react-app` starter template
- The super-popular mapping library called **Leaflet** (say goodbye to `iframes` 👋!)
- We'll be saving map data in a format called [GeoJSON](https://geojson.org/) format
#   A Little Overview About Maps


Mapping applications are in essence like a JAMstack app, they have:

- **JavaScript** (for running the mamping library)
- **API** (accessing the map API for imagery)
- **Markup** (the final output of our app will be HTML)

In this course, we'll be a building restaurant locator 💪 complete with popups, delivery radius and the ability to add a marker for your current location.

🤓 Curious about JAMStack, [learn more about JAMStack here](https://jamstack.wtf/).

# Group 1 - Your First Map

 
we'll add a map to our app using [Mapbox](https://www.mapbox.com/) 💪

🤓 [Mapbox documentation](https://docs.mapbox.com/help/how-mapbox-works/)

We will also add our first map marker (the little pin that you can find in Google maps, for example)

🤓 Mapping is hard. If you're interested in maps and want to know more about the struggles of representing Earth in a 2D map, check out the 6-min video [Why all world maps are wrong](https://www.youtube.com/watch?v=kIID5FDi2JQ) by Vox. Super-quick summary of the struggle: The surface of a sphere cannot be represented as a plane without some form of distortion.

🤓 or if you are a West Wing fan (guilty!) You can watch this [Why are we changing maps?](https://www.youtube.com/watch?v=eLqC3FNNOaI) clip instead.

#   Adding Your First React Leaflet Map to a New React Application
 
This  consists of two exercises:

- adding the two dependencies to our project
- adding a map to the project

The two main components of a React Leaflet map will be:

- A `<Map/>` component
- A `<TileLayer />` component

We'll do most of our work in the following two files:

- `App.js`
- `App.css`

🤓 [Leaflet](https://leafletjs.com/) is the leading open-source JavaScript library for mobile-friendly interactive maps (weighing just about 38 KB of JS)
# Install leaflet and react-leaflet

 

Run:

`npm install leaflet react-leaflet # npm` OR

`yarn add leaflet react-leaflet # Yarn`

to add the two dependencies to our project

👍 Note: if you are coding along you should be installing the two dependencies in: `02 - Adding Your First React Leaflet Map to a New React Application` directory

👍 Note: React-Leaflet provides an abstraction of Leaflet as React components (For example: `Map`, `Marker`, `Popup`, `TileLayer`). In other words, even though we'll be using React, you'll still need the base Leaflet library to go along with React-Leaflet.

# Adding a New Map to the Search Page
 

Time to write some code!

Inside `App.js` we'll be importing the two components we mentioned earlier!

`import {Map, TileLayer} from 'react-leaflet'`

Replace the `h1` with the `<Map>` component. We'll add some props next (the coordinates and the zoom level).

Remember, the map won't display until we add a `<TileLayer>` component, which is responsible for all the map imagery.

Your code should look similar to this:

```js
// coordinates for Washington city, but you can add your own
<Map center={[38.907132, -77.036546]} zoom={12}>
  <TileLayer
    // we'll be using OpenStreetMap (we'll change this in the later lessons)
    url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
    // don't forget to attribute them!
    attribution='&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
  />
</Map>
```

🤓 You can read more about [Zoom levels here](https://leafletjs.com/examples/zoom-levels/)

👍 Note that the `<Map>` component creates its own `<div>` container for the map, it does not get attached to an existing node.

At this point the map loads - but it looks broken 😢. Let's fix that!

Start by importing the Leaflet CSS:

`import "leaflet/dist/leaflet.css";`

Last thing is giving the map wrapepr `div` a height and a width (like with `img` elements.)

```js
.leaflet-container {
  width: 100%;
  height: 100%;
}
```
# Style leaflet Map using CSS
 

Remove the extra padding from the `main` element to give the map more space to shine!

```js
main {
  padding: 0;
}
```


#   Customizing Your React Leaflet Map with a Mapbox Basemap Style
 

Time to customize our map with Mapbox Studio 💪!

To do that, we'll need an API Key.

To generate a custom map, you'll need the following 3 things:

1. A map style (from Mapbox Studio)
2. The ID of that map style
3. API Key (saved in an `env` variable is best)

🤓 Did you know that while Mapbox.com is a commercial platform, most of their projects are open source? In Mapbox you are essentially paying for data hosting, servers and API access.

🤓 List of [Mapbox' Open Source Projects](https://github.com/mapbox)

# Create a Mapbox Account
 
Create a free [Mapbox account](https://www.mapbox.com/).

🤓 Mapbox is a type of GIS (Geographic Information System)

# Creating a Map Style in Mapbox
 
Once logged in, go to studio.mapbox.com (you can also select Studio from the dropdown menu - click on your Account icon).

Click **New Style** and select your preferred map options (colors, styles).

# Creating an API key in Mapbox

 
In Mapbox account dashboard (account.mapbox.com) click **Create a Token**.

Give it a name and leave the checked options as they are.



# Configure a Mapbox Endpoint for our Map Style



 
 

```js
REACT_APP_MAPBOX_API_KEY = "[API Key]";
REACT_APP_MAPBOX_USERID = "[Mapbox User ID]";
REACT_APP_MAPBOX_STYLEID = "[Mapbox Map Style ID]";
```


# Customize Our Map with Our Map Style Endpoint
 

The Mapbox `GET` request requires a couple of variables:

`https://api.mapbox.com/styles/v1/{username}/{style_id}/tiles/{tilesize}/{z}/{x}/{y}{@2x}`

Let's swap those placeholders with our information:

- Replace `{username}` with your Mapbox account name (make sure to remove the curly braces too!)
- Replace `{style_id}` with your custom map id (from Mapbox Studio)
- Replace `{tilesize}` with 256.
- Remove the curly brackets around `{@2x}`
- Finally, append your access_token which you'll source from you .env file:
  `?access_token=${process.env.REACT_APP_MAPBOX_API_KEY}`



# Using Our Tile Endpoint for Our Map
 
Copy the URL endpoint and replace the `url` prop inside the <TileLayer> component(in `App.js`)

Add Mapbox to the attribution:

```js
attribution =
  'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>';
```

👍 Restart your server (with `yarn develop`) and marvel at your customized map!


# Create a New Basemap Style
 
Move the `username` and `style_id` to `.env.shared` file as well.

The final url endpoint should like this:
{`https://api.mapbox.com/styles/v1/${process.env.REACT_APP_MAPBOX_USERID}/${process.env.REACT_APP_MAPBOX_STYLEID}/tiles/256/{z}/{x}/{y}@2x?access_token=${process.env.REACT_APP_MAPBOX_API_KEY}`}

👍 To make the endpoint a little bit easier to read, you can extract the variables like this:

```js
const MAPBOX_API_KEY = process.env.REACT_APP_MAPBOX_API_KEY;
const MAPBOX_USERID = process.env.REACT_APP_MAPBOX_USERID;
const MAPBOX_STYLEID = process.env.REACT_APP_MAPBOX_STYLEID;
```

Then your `url` endpoint shoud look like this:

```js
url={`https://api.mapbox.com/styles/v1/${MAPBOX_USERID}/${MAPBOX_STYLEID}/tiles/256/{z}/{x}/{y}@2x?access_token=${MAPBOX_API_KEY}`}
```



# Create an Environment Variable for the API Key
 
Time to get creative! Create a new style in Mapbox studio.

Replace the `style_id` in the `.env.shared` file, restart the server and test out the new map.



#  Adding a Marker to a Map to Point to a Location with React Leaflet

 

Time to add our first location to the map! For that, we'll need **latitude** and **longitude**of that location.

🤓 A quick refresher, [what are latitude and longitude?](https://simple.wikipedia.org/wiki/Geographic_coordinate_system)(and is it possible to spell those two words correctly?😅)

Then we'll add the **marker** - that's the pin that _marks_ the location on the map.

We'll also add a **popup** with more information.

👍 Both `<Marker>` and `<Popup>` components conveniently ship with react-leaflet library.


# Find Our Favorite Location

  
To get the coordinates of a specific location, find the location on Google maps, right-click, select **What's here** and copy the coordinates.

For example: 38.891652, -74.026070  


# Add a Marker Component with our Location
 
  

First, import the two components to our `App.js`.

Our react-leaflet imports should like this:

`import { Map, TileLayer, Marker, Popup } from "react-leaflet";`

After the TileLayer component, but still inside the Map component, add the Marker with the **position** prop and the coordinates that you copied in the previous lesson.
`<Marker position={[38.888369, -77.019900]} />`

# Fix a Library Conflict so our Marker Image Shows

 

Let's fix the `data:image/p` console error from the last video, with a workaround.

Import `useEffect` React hook:

`import React, {useEffect } from "react";`

Add the following (before the `return` statement):

```js
useEffect(() => {
  delete L.Icon.Default.prototype._getIconUrl;

  L.Icon.Default.mergeOptions({
    iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
    iconUrl: require("leaflet/dist/images/marker-icon.png"),
    shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
  });
}, []);
```

This is a code snippet copied from a GitHub issue - you're not supposed to just come up with the solution yourself.

Since we are using Leaflet here directly (the **L** from above), we'll have to import it to our file as well:

`import L from 'leaflet';`

The marker should now be there!

# Add a Popup Component to Display the Name of our Location

 

Ok, so the marker now neatly shows on the map, but what does the marker stand for?

We've already imported the Popup component in one of the previous videos. Nest the Popup component inside the Marker, add some descriptive text and voila!

```js
<Marker position={[38.888369, -77.0199]}>
  <Popup>Smithsonian National Air and Space Museum</Popup>
</Marker>
```

👍 Click on the marker to test that the popup works.


# Add a Second Marker for your Second Favorite Location
 

Practice time!

Add a second location by repeating the steps from before (Google maps -> What's here -> copy coordinates).

Then add another set of Marker + Popup components.

👍 Start by copying our first duo, then replaced the required information (mainly the coordinates, and the popup text)

# Group 2 - Managing Map Data
 
In this  we'll cover:

- managing leaflet state with React hooks
- What is GeoJSON?
- how to add more data to our popups!


#   Managing Leaflet State in a React App with Hooks

 

Time for some React Hooks!

We'll be using `useRef` to access the Leaflet API directly.

We'll fire up the `ref` with `useEffect` - timing it **after** the component was rendered!

We'll also be creating a new marker instance!

Then a quick debugging session demonstrating how the state can get out of sync (between Leaflet and the app).


# Adding a ref to Our Map Component

  

Import `useRef` along with your other React imports.

Define it as:

`const mapRef = useRef();`

And then apply the ref prop to the Map component:

`ref={mapRef}`

Sweet!


# Accessing our Leaflet Map Instance Inside a React useEffect Hook

 
We want to access our Map component via the `ref` prop with `useEffect`.

```js
useEffect(() => {
  console.log(mapRef.current);
}, [mapRef]);
```

Test that it works!

👍 Note: this will be the second `useEffect` in our App.js.


# Use our Leaflet Map Instance to re-add our Marker to the Map

 

Let's do some destructuring and take what we need from our `ref`.

```js
const { current = {} } = mapRef;
const { leafletElement: map } = current;
```

We also want to exit (`return`) if a map doesn't exist.

`if ( !map ) return;`

Now for the tricky bit!

Create a new marker instance and copy the existing coordinates (the one from the existing Marker component).
For example: `const marker = L.marker([38.888369, -77.019900])`

Add this marker to the map:

`marker.addTo(map);`

And let's not forget the popup:

`marker.bindPopup("Smithsonian National Air and Space Museum");`

Now let's comment on the `<Marker />` component and initialize it inside the `useEffect` hook.

The final `useEffect` hook should look like this:

```js
useEffect(() => {
  const { current = {} } = mapRef;
  const { leafletElement: map } = current;

  if (!map) return;

  map.eachLayer((layer = {}) => {
    const { options } = layer;
    const { name } = options;

    if (name !== "Mapbox") {
      map.removeLayer(layer);
    }
  });

  const marker = L.marker([38.888369, -77.0199]);

  marker.bindPopup("Smithsonian National Air and Space Museum");
  marker.addTo(map);
}, [mapRef]);
```

👍 We can now also remove the Marker and Popup Leaflet imports since we won't be using them.


# Review a Simple Example of Mismanaged State

 

A quick reminder to comment out (or delete) the initial Marker/Popup combo:

```js
<Marker position={[38.888369, -77.0199]}>
  <Popup>Smithsonian National Air and Space Museum</Popup>
</Marker>
```

👍 Instead, we'll be using the ones we created programmatically in the previous video.


# Recreate the Marker from our Second Favorite Location

 

Let's reinforce the process of creating a marker + popup programmatically.

👍 You can have as many markers on a map, but make sure you save them under different variable names.

You'll need to do three things:

1. Initiate a new `L.marker`
2. bind a new popup
3. add both to the map

Like so:

```js
const markerExample2 = L.marker([38.123123, -77.123123]);

marker.bindPopup("This is my super cool marker");
marker.addTo(map);
```

# Create Your First GeoJSON Document and Add Restaurant Locations to the Map
 

Instead of adding coordinates manually, we want to load them using a GeoJSON file.

**What is GeoJSON?** It's a JSON document with a specific structure.

This is an example GeoJSON object:

```js
var geojsonFeature = {
  type: "Feature",
  properties: {
    name: "Coors Field",
    amenity: "Baseball Stadium",
    popupContent: "This is where the Rockies play!",
  },
  geometry: {
    type: "Point",
    coordinates: [-104.99404, 39.75621],
  },
};
```

We'll be using [GeoJSON.io](https://geojson.io/#map=2/20.0/0.0) to generate aGeoJSON document.

🤓 You can read more about [GeoJSON here](https://leafletjs.com/reference-1.6.0.html#geojson)


# Understanding the Basics of GeoJSON

 

[GeoJSON](https://geojson.org/) is a JSON document intended specifically for handling map data.

🤓 Specifically:
"GeoJSON supports the following geometry types: Point, LineString, Polygon, MultiPoint, MultiLineString, and MultiPolygon. Geometric objects with additional properties are Feature objects. Sets of features are contained by FeatureCollection objects."

👍 Note GeoJSON is supposed to be an object, and not an array of objects.

🤓 A handy [GeoJSON linter](https://geojsonlint.com/)

# Using geojson.io to Create Your First GeoJSON Document

 

You can generate a GeoJSON object on geojson.io by clicking on the marker icon and placing it on the map. This will create a new `FeatureCollection`, its features, including the coordinates and other data.

For example:

```js
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {},
      "geometry": {
        "type": "Point",
        "coordinates": [
          -93.2330274581909,
          32.69994010385839
        ]
      }
    }
  ]
}
```

👍 If we add another marker, we'll be ading another feature to the FeatureCollection.



# Manually Add a New Restaurant Location to the GeoJSON Document

 

Instead of placing a marker on the map, we can also create a GeoJSON object by inputting the coordinates manually.

👍 Some mapping interfaces put longitude first and latitude last, others swap the two - make sure to learn which order is correct!


# Create a New GeoJSON File and Import it into the App
 
 

Our Launch app is still rather _dumb_, let's fix that by loading GeoJSON data, instead of inputting the coordinates manually.

In the `src` directory, create a new **data** folder. Inside create a `locations.json` file.

Copy the data from geojson.io to the `locations` file and then import the `locations` file to `App.js`.

Like so:

`import locations from './data/locations.json';`


# Add GeoJSON Location Data to the Map

 

Clean-up time! 🧹

Remove the three lines defining and adding the marker data (in `App.js`).

Replace them with GeoJson locations data - which the Leaflet library (`L`) will intuitively know how to handle.

`const geoJson = new L.GeoJSON(locations);`

But we still need to add it to our map:

`geoJson.addTo(map);`

👍 Note: we'll be adding the popup data in one of the following videos.
# Add Another Location to the Map

 

For extra practice, let's add another location to the GeoJSON object.

Remember, there are two ways of doing it:

1. search for a location on GeoJSON.io and drag a marker to the location (to get all the data)
2. copy an existing `feature` and replace the latitude and longitude

#  - Add Restaurant Info to GeoJSON Documents and Display it in a Tooltip on the Map

 
In this lesson we'll learn:

- how to add custom data to our GeoJSON object
- how to add popups to our GeoJSON markers
- how to add our GeoJSON data to the popups

We will also do some CSS styling to make the map feel truly ours! 🌈




# Updating our GeoJSON Data to Include Restaurant Information

 
 

Let's add some custom properties to our GeoJSON object.

Add a new `properties` property and store the custom values in an object, like so:

```js
"properties": {
  "name": "DC Pizza",
  "delivery": true,
  "phone": "(202) 331-1800",
  "website": "http://www.dcpizzaonline.com/",
  "tags": [
    "Pizza",
    "Wings",
    "Sandwiches",
    "Salads"
  ]
}
```

👍 Make sure to add the same properties object (with different values. of course) to all the locations.



# Adding Popups to all of our Markers

 
Time to add popups to our locations.

We'll be looping through the features (locations) in our object with a `onEachFeature` function.

As a first step, let's try to `console.log` the name of each restaurant:

```js
const geoJson = new L.GeoJSON(locations, {
  onEachFeature: (feature = {}, layer) => {
    const { properties = {} } = feature;
    const { name } = properties;
    console.log("name: ", name);
  },
});
```

Once that's working, we can add it the to the popup, by adding the following three lines to the geoJSON function:

```js
const popup = L.popup();
popup.setContent(name);
layer.bindPopup(popup);
```

# Adding Restaurant Information to our Popups

 
Let's destructure the rest of the information from our data object:

`const { name, delivery, tags, phone, website } = properties;`

In the `setContent` function we are able to display HTML (and not just a simple variable, like `name` that we did previously).

We need to wrap the HTML inside a **template literal** (the backticks ``).

Let's add the following html:

```js
const html =
  `<div">
    <h3>${name}</h3>
    <ul>
      <li>
        ${tags.join(", ")}
      </li>
      <li>
        <strong>Delivery:</strong> ${delivery ? "Yes" : "No"}
      </li>
      <li>
        <strong>Phone:</strong> ${phone}
      </li>
      <li>
        <strong>Website:</strong> <a href="${website}">${website}</a>
      </li>
    </ul>
  </div>
`;
```


# Update the Styles of our Popups
 
Our popups can be styled just like any other HTML element. Let's start by adding classes and then add styling to the `App.css` file.

Below is the example styling by Colby.

```css
.restaurant-popup h3 {
  font-size: 1.4em;
  margin-bottom: 0.4em;
}

.restaurant-popup ul {
  padding: 0;
  list-style: none;
  margin: 0;
}

.restaurant-popup li {
  margin-bottom: 0.4em;
}

.restaurant-popup li:last-child {
  margin-bottom: 0;
}
```
# Change the Background Color of the Popup
 

We can add some additional styling by overriding the default styling from the default `leaflet-popup` class.

The sky is the limit! 🌈

# Add Another Restaurant Attribute
 
Let's practice our GeoJSON wrangling by adding a new attribute of `vegan: true` for vegan-friendly locations.

First, let's destructure the `vegan` attribute from the `properties` object.

Then inside the HTML popup, we can add another `li` with:

```HTML
<li>
  <strong>Vegan Friendly:</strong> ${vegan ? 'Yes' : 'No'}
</li>
```


# Group 3 - Customizing Your Map

 

In this group, we'll:

1. add restaurant delivery zones (with a delivery radius)
2. replace the default markers (the location pins) with custom images
3. use Geolocation API to find locations closeby

#  Add Restaurant Delivery Zones to Map with Shaded Regions

 

To indicate a delivery radius, we'll need to add it (as a number in meters) to our ever-growing GeoJSON object.

We'll also add the shading on the radius to make it more prominent. We'll make sure to only show the radius of the location that we are currently hovering over - otherwise, chaos!

We'll also learn how to add some custom styling to the delivery radius.


# Adding a Delivery Radius to our Restaurant Data

 
 

Add a `deliveryRadius` property to the GeoJSON object.

👍 It makes sense to add `deliveryRadius` to every object that offers `delivery`.

Destructure it the `properties` attribute.

Test that everything works with a trusty `console.log`:

```js
if (deliveryRadius) {
  console.log(deliveryRadius);
}
```


# Using the Delivery Radius to add a Shaded Circle to the Map

 

Let's add a circle to signify our delivery radius (but only for locations that support delivery).

First, we'll need to destructure `geometry` from feature as well as `coordinates` from geometry.

Like so:

```js
const { properties = {}, geometry = {} } = feature;
const { coordinates } = geometry;
```

Add the circle to the map with the following code:

```js
let deliveryZoneCircle;

if (deliveryRadius) {
  // 👍 make sure to add reverse since the coordinates in GeoJSOn are stored backwards to what Leaflet expects
  deliveryZoneCircle = L.circle(coordinates.reverse(), {
    radius: deliveryRadius,
  });
  //  Don't forget to add the circle to the map.
  deliveryZoneCircle.addTo(map);
}
```




# Only Showing the Delivery Radius on Marker Hover
 

The overlapping delivery circles are a bit confusing - imagine if we added more locations to the map!

Let's only display the border radius (not to be confused with CSS `border-radius` 😅) when hovering over the marker (on `mouseover`).

Similarly, let's Remove the radius when we're done hovering (on `mouseout`)

```js
layer.on("mouseover", () => {
  if (deliveryZoneCircle) {
    deliveryZoneCircle.addTo(map);
  }
});

layer.on("mouseout", () => {
  if (deliveryZoneCircle) {
    deliveryZoneCircle.removeFrom(map);
  }
});
```


# Change the Color of the Delivery Zone

 

We can change the color of the delivery radius circle by adding the `color` option to the radius:

```js
deliveryZoneCircle = L.circle(coordinates.reverse(), {
  radius: deliveryRadius,
  color: "red",
});
```


#  - Customize Restaurant Location Markers with Custom Images

 

Time to spice up our GeoJSON configuration!

We'll be:

- adding markers programmatically (will I ever learn how to spell this word correctly?! 😅)
- swapping the default marker images for our own
- we'll need to add a custom shadow as well (to get that nice 3D effect)




# Recreate Restaurant Markers with GeoJSON Configuration Option

 
 
We'll be replacing the default marker with `utensils-marker` found in the shared assets folder.

First, let's add a new property to our GeoJSON object (inside `App.js`) called `pointToLayer`.

Initialize the markers with:

```js
  pointToLayer: (feature, latlng) => {
    return L.marker(latlng);
  },
```

# Replace the Default Location Markers with a Custom Image

 

We'll add a custom marker by adding an options object to `pointToLayer`.

First import the custom marker image at the top of the file:
`import utensilsIcon from './assets/shared/utensils-marker.png';`

Now let's add it to the options object:

```js
pointToLayer: (feature, latlng) => {
  return L.marker(latlng, {
    icon: new L.Icon({
      iconUrl: utensilsIcon,
      // size in pixels
      iconSize: [26, 26],
      // readjust the popup to be centered around the icon 🥼
      popupAnchor: [0, -15],
    })
  });
},
```

🤓 You can read more about [Leaflet's custom icons here](https://leafletjs.com/reference-1.6.0.html#icon)

# Add the Default Shadows back to our Markers
 

Let's add that fancy marker shadow!

Import the Leaflet shadow from:

`import markerShadow from 'leaflet/dist/images/marker-shadow.png';`

Then add the two shadow configurations to the icon options object:

```js
shadowUrl: markerShadow,
// if you're using a different icon, you'll probably have to play around with the values a bit to get it right
shadowAnchor: [13, 28],
```
# Replace the Marker with Custom HTML and Style with CSS
 

Time to replace our custom icon with a Leaflet's `L.divIcon`

```js
return L.marker(latlng, {
  icon: L.divIcon(),
});
```

We can add the preferred HTML structure straight to the icon's options object:

```js
return L.marker(latlng, {
  icon: L.divIcon({
    html: "<div class="my-class">HIHIHI</div>",
  }),
});
```

And styling as well! (Apply styling to the `.my-class` CSS class)

👍 Check out [Leaflet's map examples for inspiration](https://react-leaflet.js.org/docs/en/examples)
# Lesson 10 - Use Leaflet's Geolocation API to Find Locations Near You
 

Time to improve the user experience! 🦄

From now on, we want users to add new markers to the map with a click of a button.

To make things smoother, we want to be able to check the user's location - so we can display results relevant to their geographic area.

We also want a new marker when the location is found.

The browsers won't be able to pinpoint the _exact_ location, so we'll adjust our calculation to accept a margin of error.

Lastly, to avoid any performance issues down the line, we want to clean up all event handlers that have already been used.

# Add a Marker to a Location when Clicking a Button
 
In the starter code for lesson 10 you'll find a new button for Setting the location to the National Geographic Museum. Let's copy it!

Then add the marker, using the code we're already familiar with:

```js
const marker = L.marker(locationNationalGeographic);

marker.addTo(map);
```

Once the marker has been added, we want to center it on the screen. Add the following code to achieve that:

`map.setView(locationNationalGeographic);`

# Create a Button taht Finds your Location and Navigates the Map to that Location

 

We'll add a new button for Finding the user's location with a new `onClick` function.

```js
<button onClick={handleOnFindLocation}>Find My Location</button>
```

Next, let's define the click handler:

```js
function handleOnFindLocation() {
  const { current = {} } = mapRef;
  const { leafletElement: map } = current;

  map.locate({
    setView: true,
  });
}
```

👍 You'll have to click "allow" when prompted by the browser.
# Use the Browser's Location to Add a Marker to the Map
 

We've located the user's location, but how do we automatically add a marker to it?

We can listen for when the `locate` function was fired (using the `useEffect` hook)
`map.on('locationfound', handleOnLocationFound);`

Now let's define `handleOnLocationFound`:

```js
function handleOnFindLocation() {
  const { current = {} } = mapRef;
  const { leafletElement: map } = current;

  map.locate({
    setView: true,
  });
}
```

A new marker will automatically be added when clicking the **Find My Location** button.

🤓 You can find other [Map events here](https://leafletjs.com/reference-1.6.0.html#map-event).
# Add a Circle to the Map Designing the Accuracy of the Browser's Location
 

If we `console.log` the `event` inside `handleOnLocationFound`, you'll notice the `accuracy` property (the radius in meters).

Let's demonstrate this accuracy radius with a circle:

Inside `handleOnLocationFound` add:

```js
const radius = event.accuracy;

const circle = L.circle(latlng, {
  radius,
  // use any color, but best if it's different from the color of delivery zones
  color: "#26c6da",
});
// don't forget to add any new shapes to the map!
circle.addTo(map);
```
# Clean up Location Event handler Resources when Page Unmounts
 

Clean-up time! 🧹

Inside the `locationfound` event handler add:

```js
return () => {
  map.off("locationfound", handleOnLocationFound);
};
```

**Aaaaaaaaaaaaaaaaaaaaaand, you did it!  🎉🎆🍾🎊💃**
