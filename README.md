# retina.js [![Build Status](https://secure.travis-ci.org/imulus/retinajs.png?branch=master)](http://travis-ci.org/imulus/retinajs)

### JavaScript, LESS and SASS helpers for rendering high-resolution image variants

retina.js makes it easy to serve high-resolution images to devices with retina displays

[![Build Status](https://secure.travis-ci.org/imulus/retinajs.png?branch=master)](http://travis-ci.org/imulus/retinajs)

## How it works

When your users load a page, retina.js checks each image on the page to see if there is a high-resolution version of that image on your server. If a high-resolution variant exists, the script will swap in that image in-place.

The script assumes you use Apple's prescribed high-resolution modifier (@2x) to denote high-resolution image variants on your server. It is also possible to override this by manually specifying the URL for the @2x images using `data-at2x` attributes.

For example, if you have an image on your page that looks like this:

```html
<img src="/images/my_image.png" />
```

The script will check your server to see if an alternative image exists at `/images/my_image@2x.png`

However, if you have:

```html
<img src="/images/my_image.png" data-at2x="http://example.com/my_image@2x.png" />
```

The script will use `http://example.com/my_image@2x.png` as the high-resolution image. No checks to the server will be performed.

## How to use

### JavaScript

The JavaScript helper script automatically replaces images on your page with high-resolution variants (if they exist). To use it, download the script and include it at the bottom of your page.

1. Place the retina.js file on your server
2. Include the script on your page (put it at the bottom of your template, before your closing \</body> tag)

``` html
<script type="text/javascript" src="/scripts/retina.js"></script>
```

You can also exclude images that have no high-res version. Simply add the `data-no-retina` attribute.


``` html
<img src="/path/to/image" data-no-retina />
```

###LESS & SASS

The LESS &amp; SASS CSS mixins are helpers for applying high-resolution background images in your stylesheet. You provide it with an image path and the dimensions of the original-resolution image. The mixin creates a media query specifically for Retina displays, changes the background image for the selector elements to use the high-resolution (@2x) variant and applies a background-size of the original image in order to maintain proper dimensions. To use it, download the mixin, import or include it in your LESS or SASS stylesheet, and apply it to elements of your choice. The SASS versions require you pass the extension separately from the path.

*Syntax:*

``` less
.at2x(@path, [optional] @width: auto, [optional] @height: auto);
```

``` scss
@include at2x($path, [option] $ext: "jpg", [optional] $width: auto, [optional] $height: auto);
```

*Steps:*

1. LESS - Add the .at2x() mixin from retina.less to your LESS stylesheet (or reference it in an @import statement)
SASS - Add the @mixin at2x() from retina.scss or retina.sass to your SASS stylesheet (or reference it in an @import)
2. LESS - In your stylesheet, call the .at2x() mixin anywhere instead of using background-image
SASS - In your stylesheet, call @include at2x() anywhere instead of using background-image

This:

``` less
.logo {
  .at2x('/images/my_image.png', 200px, 100px);
}
```

``` sass
.logo {
  @include at2x('/images/my_image', png, 200px, 100px);
}
```

Will compile to:

``` css
.logo {
  background-image: url('/images/my_image.png');
}

@media all and (-webkit-min-device-pixel-ratio: 1.5) {
  .logo {
    background-image: url('/images/my_image@2x.png');
    background-size: 200px 100px;
  }
}
```
