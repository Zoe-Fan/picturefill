# Picturefill

## Overview 

A JavaScript polyfill to implement Responsive Images using the [proposed picture element](http://www.w3.org/TR/2013/WD-html-picture-element-20130226/).

if you have ever designed a responsive site using a framework like bootstrap or foundation, you know that if you want different images at different breakpoints you end up downloading one set of images then hiding them.

This JavaScript polyfill allows you to implement responsive images quickly and easily on your site.



## How to use

First include the picturefill in the head of your html similar to 

```html
	<html>
		<head>
			<script src="picturefill.js"></script>
		</head>
```

Whenever you want to include a picture on your site you use the following syntax 

```html
	<picture>
		<source srcset="images/small.jpg"/>
	</picture>
```

This works exactly like a normal image tag.

Different images for desktop and smartphones is acheived by using media queries. **Assuming that a tablet is anything wider than 480px and a desktop is something wider than 992px you would create the following picture tag.


```html
	<picture>
		<source srcset="images/mobile.jpg"/>
		<source srcset="images/tablet.jpg" media="(min-width: 480px)"/>
		<source srcset="images/desktop.jpg" media="(min-width: 992px)"/>
	</picture>
```

To handle browsers that don't support JavaScript we add a noscript tag and insert a standard image to be loaded

```html
	<picture>
		<source srcset="images/mobile.jpg"/>
		<source srcset="images/tablet.jpg" media="(min-width: 480px)"/>
		<source srcset="images/desktop.jpg" media="(min-width: 992px)"/>
		<!-- Fallback content for non-JS browsers. Same img src as the initial, unqualified source element. -->
		<noscript><img src="images/mobile.jpg" alt="Alternative text for your image"></img></noscript>
	</picture>
```

### Alt, Class and Style attributes

In order to add attributes to the img tag that is finally generated you add them to the picture tag with a prefix of data- to stop the browser from styling your page twice. These will be copied over to the resulting image.

For example

```html
	<picture data-alt="Alternative text for your image" data-class="hero-image">
		<source srcset="images/mobile.jpg"/>
		<source srcset="images/tablet.jpg" media="(min-width: 480px)"/>
		<source srcset="images/desktop.jpg" media="(min-width: 992px)"/>
		<!-- Fallback content for non-JS browsers. Same img src as the initial, unqualified source element. -->
		<noscript><img src="images/mobile.jpg" alt="Alternative text for your image" class="hero-image"></img></noscript>
	</picture>
```

When run on a tablet becomes

```html
	<picture data-alt="Alternative text for your image" data-class="hero-image">
		<source srcset="images/mobile.jpg"/>
		<source srcset="images/tablet.jpg" media="(min-width: 480px)">
			<img src="images/tablet.jpg" alt="Alternative text for your image" class="hero-image">
		</source>
		<source srcset="images/desktop.jpg" media="(min-width: 992px)"/>
		<!-- Fallback content for non-JS browsers. Same img src as the initial, unqualified source element. -->
		<noscript><img src="images/mobile.jpg" alt="Alternative text for your image" class="hero-image"></img></noscript>
	</picture>
```


### Retina Display

The easiest way to add retina images is to use a media query to target retina displays. Using the existing example we will add a new media query *(min-device-pixel-ratio: 2.0)* this media query only matches on retina displays (iPad, iPhone etc...) 

So we now end up with the following picture tag


```html
	<picture data-alt="Alternative text for your image" data-class="hero-image">
		<source srcset="images/mobile.jpg"/>
		<source srcset="images/mobile_2x.jpg" media="(min-device-pixel-ratio: 2.0)"/>
		<source srcset="images/tablet.jpg" media="(min-width: 480px)"/>
		<source srcset="images/tablet_2x.jpg" media="(min-width: 480px and min-device-pixel-ratio: 2.0)"/>
		<source srcset="images/desktop.jpg" media="(min-width: 992px)"/>
		<source srcset="images/desktop_2x.jpg" media="(min-width: 992px and min-device-pixel-ratio: 2.0)"/>
		<!-- Fallback content for non-JS browsers. Same img src as the initial, unqualified source element. -->
		<noscript><img src="images/mobile.jpg" alt="Alternative text for your image" class="hero-image"></img></noscript>
	</picture>
```


* Note: Supporting this many breakpoints increases implementation and maintenance time, so use this technique sparingly.

