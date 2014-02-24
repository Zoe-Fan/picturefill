# Picturefill
A JavaScript polyfill to implement Responsive Images using the [proposed picture element](http://www.w3.org/TR/2013/WD-html-picture-element-20130226/).


* Author: Scott Jehl (c) 2012 (new proposal implemented by Barry Latimer Jansepar)
* License: MIT/GPLv2

**Demo URL:** [http://jansepar.github.com/picturefill/](http://jansepar.github.com/picturefill/)

**Draft Specification:** [http://picture.responsiveimages.org/](http://picture.responsiveimages.org/)


## How to use

First include the picturefill in the head of your html similar to 

```html
	<html>
		<head>
			<script src="picturefill.js"></script>
		</head>
```

Then whenever you want to include a picture on your site you can do the following 

```html
	<picture>
		<source srcset="images/small.jpg"/>
	</picture>
```

This will work exactly like a normal image tag, and will add the image 
Mark up your responsive images like this.

```html
	<span data-picture data-alt="A giant stone face at The Bayon temple in Angkor Thom, Cambodia">
		<span data-srcset="images/small.jpg"></span>
		<span data-srcset="images/medium.jpg" data-media="(min-width: 400px)"></span>
		<span data-srcset="images/large.jpg" data-media="(min-width: 800px)"></span>
		<span data-srcset="images/extralarge.jpg" data-media="(min-width: 1000px)"></span>

		<!-- Fallback content for non-JS browsers. Same img src as the initial, unqualified source element. -->
		<noscript><img src="external/imgs/small.jpg" alt="A giant stone face at The Bayon temple in Angkor Thom, Cambodia"></img></noscript>
	</span>
```

Or when using `sizes`:

```html
    <span data-picture data-alt="Obama with soldiers">
        <span data-sizes="(max-width: 30em) 100%, (max-width: 50em) 75%, 50%"
                 data-srcset="images/pic-small.png 400w, images/pic-medium.png 800w, images/pic-large.png 1200w">
		<noscript><img src="images/pic-small.jpg"></noscript>
    </span>
```

### Explained...

Notes on the markup above...

* The `span[data-picture]` element's `alt` attribute is used as alternate text for the `img` element that picturefill generates upon a successful source match.
* The `span[data-picture]` element can contain any number of `span` elements. The above example may contain more than the average situation may call for.
* Each `span[data-srcset]` element must have a `data-srcset` attribute specifying the image path.
* It's generally a good idea to include one source element with no `media` qualifier, so it'll apply everywhere - typically a mobile-optimized image is ideal here.
* The `span[data-srcset]` can take in a single image (like "images/small.jpg"), or an array of images ("images/pic-small.png 0.5x, images/pic-medium.png 1x, images/pic-large.png 1.5x).
* The `span[data-sizes]` attribute is available to specify the size of the image at different breakpoints. Then, in `data-srcset`, an array of images at different widths are supplied, and based on the breakpoints defined in `data-sizes`, the appropriate image will be chosen. So in the example above, "images/pic-small.png 400w, images/pic-medium.png 800w, images/pic-large.png 1200w" will be converted to "images/pic-small.png 0.5x, images/pic-medium.png 1x, images/pic-large.png 1.5x" if the width of the device (in css pixels) was 800px;
* Each `[data-srcset]` element can have an optional `[data-media]` attribute to make it apply in specific media settings. Both media types and queries can be used, like a native `media` attribute, but support for media _queries_ depends on the browser (unsupporting browsers fail silently).
* The `matchMedia` polyfill (included in the `/external` folder) is necessary to support the `data-media` attribute across browsers (such as IE9), even in browsers that support media queries, although it is becoming more widely supported in new browsers.
* The `noscript` element wraps the fallback image for non-JavaScript environments, and including this wrapper prevents browsers from fetching the fallback image during page load (causing unnecessary overhead). Generally, it's a good idea to reference a small mobile optimized image here, as it's likely to be loaded in older/underpowered mobile devices.


### How the `img` is appended

Upon finding a matching `span[data-src]` element, picturefill will generate an `img` element referencing that `span`'s `data-src` attribute value and append the `img` to the active, matching `span[data-src]` element. This means you can target CSS styles specific to the active image based on the breakpoint that is in play, perhaps by adding a class to each span. For example, if you have the following markup...

```html
	<span class="picture" data-picture data-alt="A giant stone face at The Bayon temple in Angkor Thom, Cambodia">
		<span class="sml" data-src="small.jpg"></span>
		<span class="med" data-src="medium.jpg"     data-media="(min-width: 400px)"></span>
		<span class="lrg" data-src="large.jpg"      data-media="(min-width: 800px)"></span>
````

...then you could write styles specific to each of the images, which may be handy for certain layout situations.

```css
	.picture .sml img { /* Styles for the small image */ }
	.picture .med img { /* Styles for the medium image */ }
	.picture .lrg img { /* Styles for the large image */ }
````


### HD Media Queries

Picturefill natively supports HD(Retina) image replacement.  While numerous other solutions exist, picturefill has the added benefit of performance for the user in only being served one image.

* The `data-media` attribute supports [compound media queries](https://developer.mozilla.org/en-US/docs/CSS/Media_queries), allowing for very specific behaviors to emerge.  For example, a `data-media="(min-width: 400px) and (min-device-pixel-ratio: 2.0)` attribute can be used to serve a higher resolution version of the source instead of a standard definition image. Note you currently also need to add the `-webkit-min-device-pixel-ratio` prefix (e.g. for iOS devices).

```html
	<span data-picture data-alt="A giant stone face at The Bayon temple in Angkor Thom, Cambodia">
		<span data-src="small.jpg"></span>
		<span data-src="small_x2.jpg"      data-media="(min-device-pixel-ratio: 2.0)"></span>
		<span data-src="medium.jpg"        data-media="(min-width: 400px)"></span>
		<span data-src="medium_x2.jpg"     data-media="(min-width: 400px) and (min-device-pixel-ratio: 2.0)"></span>
		<span data-src="large.jpg"         data-media="(min-width: 800px)"></span>
		<span data-src="large_x2.jpg"      data-media="(min-width: 800px) and (min-device-pixel-ratio: 2.0)"></span>
		<span data-src="extralarge.jpg"    data-media="(min-width: 1000px)"></span>
		<span data-src="extralarge_x2.jpg" data-media="(min-width: 1000px) and (min-device-pixel-ratio: 2.0)"></span>

		<!-- Fallback content for non-JS browsers. Same img src as the initial, unqualified source element. -->
		<noscript>
			<img src="small.jpg" alt="A giant stone face at The Bayon temple in Angkor Thom, Cambodia">
		</noscript>
	</span>
```

* Note: Supporting this many breakpoints quickly adds size to the DOM and increases implementation and maintenance time, so use this technique sparingly.

### Supporting IE Desktop

Internet Explorer 8 and older have no support for CSS3 Media Queries, so in the examples above, IE will receive the first `data-src`
 image reference (or the last one it finds that has no `data-media` attribute). If you'd like to serve a larger image to IE desktop
browsers, you might consider using conditional comments, like this:

```html
	<span data-picture data-alt="A giant stone face at The Bayon temple in Angkor Thom, Cambodia">
		<span data-src="small.jpg"></span>
		<span data-src="medium.jpg" data-media="(min-width: 400px)"></span>

		<!--[if (lt IE 9) & (!IEMobile)]>
		    <span data-src="medium.jpg"></span>
		<![endif]-->

		<!-- Fallback content for non-JS browsers. Same img src as the initial, unqualified source element. -->
		<noscript>
			<img src="small.jpg" alt="A giant stone face at The Bayon temple in Angkor Thom, Cambodia">
		</noscript>
	</span>
```

### Deferred loading

If picturefill is deferred until after load is fired, images will not load unless the browser window is resized.
Picturefill is intentionally exposed to the global space, in the unusual situation where you might want to defer loading of picturefill you can explicitly call window.picturefill().

## Support

Picturefill supports a broad range of browsers and devices (there are currently no known unsupported browsers), provided that you stick with the markup conventions provided.

