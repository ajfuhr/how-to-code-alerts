How to Code Your Own StreamElements Alerts
==========================================

Requirements
------------

Other than a little bit of time and some basic reading comprehension, all you need to get this going is:

- A Twitch account
- A StreamElements account connected to your Twitch account

**NOTE**: This has not been tested on Stream Labs. I believe the functionality is there, but some things may be slightly different. I have only tested this in StreamElements.

Part I: HTML
------------

HTML stands for HyperText Markup Language. It is the markup language for documents designed to be displayed within a web browser. Since StreamElements is based on loading in your overlays via a browser source, it essentially renders HTML into your broadcasting software of choice. This makes it relatively easy to design your own alerts with HTML!

Now, don't be too scared. HTML is a fairly straightforward markup language made up of (mostly) English words, or derivitives of them. HTML is made up of *tags*. These tags represent *elements* within a web page. For this guide, we will be focusing on two tags: div and span.

```
<div>
  I'm a div!
</div>

<span>
  I'm a span!
</span>
```

As you can see, each element has a opening tag and an closing tag, with contents *nested* inside of them. Opening tags begin with a '<' and end with a '>', with their keyword inbetween. Closing tags begin with a '</' and end with a '>', with the same keyword inbetween, the forward slash denoting that it is a closing tag.

We can even get crazy and nest tags within tags:

```
<div>
  <span>I'm a span nested inside of a div!</span>
</div>
```

Easy? Easy.

Now, about those alerts we want to code. Open up your StreamElements overlay dashboard and create a new overlay. Name it, I don't know, "Custom Alerts" or something that sits well with you. Once it is created, open up the overlay editor within StreamElements and confirm that you want it at 1080p. The overlay will be blank, so we'll need to add an AlertBox widget to it. Click the '+' and add that in now.

In the Layers tab to the left, select the AlertBox widget, then go to the "Position, size, and style" tab. Make it 1920x1080. We want the entire 1080p canvas.

In the Settings tab, click the gear icon to the right of the Follower alert type. Clear the default image/video and sound files, then scroll down to the "Animation length" parameter. Make this '5.5s', which will be the total time it takes for our custom alerts to play.

Finally, toggle the "Enable Custom CSS" option to on, then click "Open CSS Editor" and make your best "what in the hell does this mean" face.

This view has four tabs: HTML, CSS, JS, and Fields. HTML will be our focus for now. Click that tab, select all of the HTML code within it and press your delete key. We don't want any of that stuff because we're going to write our own!

Type the following into the now blank editor view:

```
<div>

</div>
```

This div tag, or "division" of the HTML document, will house all of our alert information. It's important. It needs a way to be identified. We'll use HTML's 'id' attribute within the opening div tag, like so:

```
<div id="alert">

</div>
```

We have a div, and its id is "alert"! It needs some more stuff, though, if we're to use this as a base for our custom alerts. It needs *children*.

Child elements are elements that are nested within another element. In our nesting example from before, the span element was the child of the div:

```
<div>
  <span>Child of div</span>
</div>
```

We need two children for our follower alert: one to show the alert's type (Follower), and one to show the name of the viewer associated with the alert. I'll save some time and write the whole thing out for you, just understand that these are *two separate child elements* and should not be nested within each other:

```
<div id="alert">
  <div id="alert-type">
    Follower
  </div>

  <div id="alert-information">

  </div>
</div>
```

See that? The alert-type div closed *before* the alert-information div opened. That makes them siblings, and children of the alert div.

We need one more thing for StreamElements to populate our new follower's name: a span with a *specific* id. It's important that you keep this id as-is. Don't make it something you like better. Just *keep this id value as-is*, or StreamElements won't know where to put your new follower's name!

```
<div id="alert">
  <div id="alert-type">
    Follower
  </div>

  <div id="alert-information">
    <span id="username-container"></span>
  </div>
</div>
```

There. The bones for our custom Follower alert are in place. Now we can begin the fun part: styling and animating the whole thing!

Part II: CSS
------------

CSS stands for Cascading Style Sheets. It is a way for web developers to describe *how* HTML elements should look when the web page is rendered. Just so you don't feel lost or overwhelmed, we'll dive into a quick overview of CSS syntax.

CSS has three parts:

- Selectors
- Attributes
- Values

A *selector* tells the browser which element you'd like to change, an *attribute* tells the browser what part of the element you want to change, and a *value* assigns the exact description you want to convey to the browser for that specific attribute. If we wanted all div elements to be 800 pixels by 800 pixels with a black background, we'd write it like this:

```
div {
  width: 800px;
  height: 800px;
  background: #000000;
}
```

Selectors are followed by a set of curly braces ({ and }), then, nested inside of those braces, are our attributes (width, height, background), and each attribute is followed by a colon (:), a space, then its value, ultimately ending each line with a semi-colon (;).


