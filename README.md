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

Let's start describing our main element, the alert div. We can use an id selector to tell the browser we only want to describe a *single* element. Id selectors in CSS start with a pound or hash symbol (#), and are followed by the HTML id for the element you want to change with no space inbetween:

```
#alert {

}
```

We want this element to be a block, placed in a specific spot with specific dimensions, and animated to slide into view the moment an alert is triggered, then slide back out of view. Go line by line and try to understand what's going on in the following code block:

```
#alert {
  display: block;
  position: absolute;
  width: 400px;
  height: 100px;
  margin: 0;
  padding: 0;
  top: 490px;
  left: -400px;
  background: #333333;
  opacity: 1.0;

  animation: slide-in 0.5s forwards, slide-out 0.5s forwards;
  animation-delay: 0s, 5s;
}
```

Essentially, we're hiding it off the bat. The attribute "left: -400px" pushes it 400 pixels off the left-most boundry of the rendered area, which matches our alert's width. We're then animating it as soon as it is rendered to slide in, waiting five seconds, then sliding it off the screen to its original location. If you go to the CSS tab within StreamElements, delete all of the default CSS, paste in the above code block, then trigger a test event for the Follower alert... nothing will happen because we haven't defined these animations yet.

We define animations by using the '@keyframes' keyword, followed by the animation name we provided in the 'animation' attribute above. Let's define our slide in and slide out animations underneath the closing curly brace from the above code block:

```
@keyframes slide-in {
  0% {
    left: -400px;
  }

  100% {
    left: 0;
  }
}

@keyframes slide-out {
  0% {
    left: 0;
  }

  100% {
    left: -400px;
  }
}
```

Test it out again by triggering a Follower event in the StreamElements overlay editor. A slate black rectangle, 400x100, will slide out from center left, wait, then slide back off screen. Let's finish up by defining how we want our alert-type text ("Follower") and the user's name to appear.

I'm thinking fades. Fades are cool. To start, we'll define the #alert-type and #alert-information elements, along with their animation properties. Brace yourself for a big block of CSS:

```
#alert-type {
  display: flex;
  position: absolute;
  width: 400px;
  height: 100px;
  margin: 0;
  padding: 0;
  top: 0;
  left: 0;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: #00acff;
  font-size: 20pt;
  font-weight: 600;
  opacity: 0.0;

  animation: fade-in 0.5s forwards, fade-out 0.5s forwards;
  animation-delay: 0.5s, 2.5s;
}

#alert-information {
  display: flex;
  position: absolute;
  width: 400px;
  height: 100px;
  margin: 0;
  padding: 0;
  top: 0;
  left: 0;
  justify-content: center;
  align-items: center;
  text-align: center;
  font-size: 18pt;
  font-weight: 300;
  color: #ffffff;
  opacity: 0.0;

  animation: fade-in 0.5s forwards, fade-out 0.5s forwards;
  animation-delay: 2.5s, 4.5s;
}
```

What we've done is place two 400x100 blocks on top of each other, centered everything horizontall and vertically, set font sizes, boldness, and color, and then... made it all invisible with the 'opacity: 0.0' attribute. We don't want to see this the second the alert is triggered, so we add in the fade-in and fade-out animations with 'animation-delay' properties set to make it all seem very fluid.

Let's define those animations now:

```
@keyframes fade-in {
  0% {
    opacity: 0.0;
  }

  100% {
    opacity: 1.0;
  }
}

@keyframes fade-out {
  0% {
    opacity: 1.0;
  }

  100% {
    opacity: 0.0;
    display: none;
  }
}
```

There! Save your work and trigger a test Follower event. The #alert div should slide in, "Follower" will fade in and out, and then the user's name will do the same, with the #alert div sliding out just as the user's name fades away.

That's it for the CSS portion! Save your overlay and let's head over to OBS!

Part III: Adding Other Alert Types
----------------------------------

Within this repo, you will see a 'src' directory. Within it, there are HTML files for each alert type in StreamElements save for merchandising alerts. I never use them, so never coded them, but you can easily add that in. Repeat what you did for the Follower alert by copying the HTML for each alert type into the editor. Remember to enable custom CSS and set each animation length in StreamElements to 5.5s. Make sure the CSS is attached to each alert type as well. I've left the full CSS code in the 'src/css' folder under 'alerts.css' for easy copying and pasting.

Some alert types, like cheers, re-subs, bits, hosts, and raids, will have a separate span element in them with and id of "amount-container" where StreamElements puts things like bit amounts, host/raid amounts, sub length, etc. Make sure you're pulling from the right source file when updating these!

Part III: OBS Setup
-------------------

Within OBS, create a browser source within any scene you want these alerts to play, copy the StreamElements URL for the overlay we just created, and paste it into the browser source's URL. Set the dimensions to 1920x1080, and make sure to check the box next to "shut down this source when inactive". You're all set!

In Closing
----------

I know, these alerts are basic as hell, but if you really want to dive into this, you can do some amazing stuff! Learn more about HTML and CSS, specifically CSS animations and HTML img tags, and you can really go wild! Let me know what you create with this, as well!