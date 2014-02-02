# Jekyll Image Tag R

> My fork of the [Jekyll Image Tag](https://github.com/robwierzbowski/jekyll-image-tag) plugin.

**Jekyll Image Tag R** is a [Jekyll][1] plugin which adds image tag functionality in [Liquid][2]. Store image presets, add classes, alt text, and any other attribute to an image's HTML, and automatically create resized images from a tag argument or YAML configuration.

## Installation

Jekyll Image Tag requires [Jekyll](http://jekyllrb.com) `>=1.0`, [Minimagick](https://github.com/minimagick/minimagick) `>=3.6`, and [Imagemagick](http://www.imagemagick.org/script/index.php).

Once you have the requirements installed, copy image_tag.rb into your Jekyll _plugins folder.

## Usage

There are two parts to Jekyll Image Tag: 

- [Liquid Tag](#liquid-tag)
- [Configuration](#configuration)

### Liquid Tag

```
{% image [preset or WxH] path/to/img.jpg [attr="value"] %}
```

The tag takes a mix of user input and pointers to configuration settings. 

#### image

Tells Liquid this is a Jekyll Image Tag.

#### preset or WxH

Optionally specify a [preset](#presets) or image dimensions. Leave blank to use the image as is.

A preset will resize the image based on the preset's width and height properties, and add any preset attributes to the image's HTML.

Image dimensions follow the format `WIDTHxHEIGHT`. Each dimension can be either a number of pixels or the keyword `AUTO`. If one dimension is `AUTO` the image will scale proportionately. Two numbers will scale and crop.

#### path/to/img.jpg

The base image. Can be a jpeg, png, or gif.

#### attribute="value"

Optionally specify any number of HTML attributes. These will be merged with any [attributes you've set in a preset](#attr). An attribute set in a tag will override the same attribute set in a preset.

### Configuration

Jekyll Image Tag stores settings in an `image` key in your _config.yml.

**Example settings**

```yml
image:
  source: assets/images/source
  output: assets/images/generated
  presets:
    users:
      attr:
        class: users
        itemprop: image
      width: 350
```

#### source

To make writing tags easier you can specify a source directory for your assets. Base images in the tag will be relative to the `source` directory. 

For example, if `source` is set to `assets/images/source`, the tag `{% image stevenson/dream.jpg alt="A night scene" %}` will look for a file at `assets/images/source/stevenson/dream.jpg`.

Defaults to the site source directory.

#### output

Jekyll Image Tag generates resized images to the `output` directory specified in your Jekyll configuration. The `generated` folder is used if none is specififed. Images are generated to the output folder before Jekyll finishes compiling and are copied into your site.

#### presets

Presets contain reusable settings for a Jekyll Image Tag. 

For example, a `users` preset might set image dimensions, a class, and some metadata attributes needed for all user pictures. If the design changes, you can edit the `users` preset and the new settings will apply to every tag that references it.

A preset name can't contain the `.`, `:`, or `/` characters.

#### attr

Optionally add a list of html attributes to insert when the preset is used.

Set the value of standalone attributes to `nil`.

An attribute set in a tag will override the same attribute set in a preset.

#### width and height

The same as [width and height](#preset-or-wxh) at the tag level, except that dimensions with an `AUTO` value are omitted. Set a single value to scale the image proportionately. Set both to scale and crop.

## Using Liquid variables and JavaScript templating

You can use liquid variables in an image tag:

```html
{% image {{ post.featured_image }} alt="our project" %}
```

If you're using a JavaScript templating library such as Handlebars.js, the templating expression's opening braces must be escaped with backslashes like `\{\{` or `\{\%`. They'll be output as normal `{{ }}` expressions in HTML:

```
{% image {{ post.featured_image }} alt="\{\{ user_name }}" %}.
```

## Managing Generated Images

Jekyll Image Tag creates resized versions of your images when you build the site. It uses a smart caching system to speed up site compilation, and re-uses images as much as possible.

Try to use a base image that is larger than the largest resized image you need. Jekyll Image Tag will warn you if a base image is too small, and won't upscale images.

By using a `source` directory that is ignored by Jekyll you can prevent huge base images from being copied to the compiled site.

Resizing images is a good first step to improve performance, but you should still use a build process to optimize site assets before deploying.

## Version History

+ 0.2.0 2014-02-02
  + First version of this fork
  + Now generates images into source then queues in Jekyll copy task
+ 0.1.2 2013-08-04
  + Bugfixes
+ 0.1.1 2013-07-13
  + Refactor, add Liquid parsing
+ 0.1.0 2013-07-14
  + Initial release

## License

[BSD-NEW](http://en.wikipedia.org/wiki/BSD_License)


[1]: http://jekyllrb.com
[2]: https://github.com/Shopify/liquid
