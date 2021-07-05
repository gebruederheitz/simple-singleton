# Wordpress Async Posts Provider

Helps you get your posts asynchronously to the frontend in your plugin or theme

Provides a simple interface ideal for custom pagination especially on hybrid 
setups using both synchronous server-side rendering and asynchronous AJAX-powered
loading of posts from a frontend script or framework using WP's own REST API.

# Installation

via composer:
```shell
> composer require gebruederheitz/wp-async-posts-provider
```


# Usage

## Using the container with default settings

Initialize the container (usually in your `functions.php`):

```php
<?php

use Gebruederheitz\WpAsyncPostsProvider\AsyncPostsProvider;

AsyncPostsProvider::getInstance()->init();
```

Make sure you have Composer autoload or an alternative class loader present.

You may pass any options provided by `ContainerSettings` as class properties in
a string-keyed array to the container to modify defaults ([see below](#available-options)):

```php
AsyncPostsProvider::getInstance()->init([
    'rendererTemplatePath' => 'template-parts/fancy/excerpt',
    'defaultPartial' => 'fancy-tile',
]);
```

Instantiating the container will register all the relevant Wordpress hooks and 
set up the REST route for use with your frontend scripts / framework.

## Available options

The following options can be set by passing the container an array with the
following keys:

| option key            | default value                | description |
| --------------------- | ---------------------------- | ----------- |
| initialPostCount      | (int) 8                      | The number of posts rendered server-side (on page 0). |
| perPage               | (int) 6                      | The number of posts per page _after_ page 0.          |
| postFilterClass       | (string) PostFilter::class   | The classes used for the various functionalities. This is where... |
| asyncPostsClass       | (string) AsyncPosts::class   | ...you can provide your own classes implementing the... |
| validatorClass        | (string) Validator::class    | ...respective interfaces to extend... |
| rendererClass         | (string) PostRenderer::class | ...or modify functionality.           |
| rendererTemplatePath  | (string) 'template-parts/content/content-excerpt' | The template path passed to `get_template_part` by the renderer. |
| defaultPartial        | (string) 'small'             | The default template name passed as the second parameter to `get_template_part` by the renderer. This can be overwritten by the `partial` request parameter. |
| route                 | (string) '/posts/load-more'  | The route used for retrieving paginated posts asynchronously. This will be prefixed with `/ghwapp/v1` and the basic WP REST path (usually `/wp/json`). |


## Basic usage

### Hybrid setup: Rendering "page zero"

If you want to deliver rendered HTML with the first couple of posts to the user,
you can use the PostFilter to automatically limit the number of posts initially 
returned by the main query on the front page (currently `is_home()`, will be 
customizable soon).  
Set the number of posts you want to be loaded initially by passing the 
`initialPostCount` option. Setting this value to `0` will disable the 
pre-filtering, and you will receive the default number of items on the main query
(which can be set through the Wordpress settings).

To determine whether or not there are any posts left to be loaded asynchronously
you can use the PostFilter's `shouldShowLoadMoreButton()` method:

```php
use Gebruederheitz\WpAsyncPostsProvider\AsyncPostsProvider;
?>
<div>
    <?php if (AsyncPostsProvider::getInstance()->getPostFilter()->shouldShowLoadMoreButton()): ?>
        <button id="load-more">Load more posts</button>
    <?php endif; ?>
</div>
```

## Usage on the front end

> todo


## Extending or modifying components

### Injecting services

> todo

### Custom filtering using PostFilter

> todo



# Development

> todo
