# Wordpress Toolbox

Wordpress tips/tricks/tools I'm gathering along the way while learning how to use it.

### Plugins

#### [Query Monitor](https://wordpress.org/plugins/query-monitor/)

>  View debugging and performance information on database queries, hooks, conditionals, HTTP requests, redirects and more. 

I found this to be very useful for debugging a variety of things such as which hooks have been triggered, which theme files are used on the current page, what conditionals match it, et cetera.

#### [Timber](https://wordpress.org/plugins/timber-library/)

> Helps you create themes faster with sustainable code. With Timber, you write HTML using Mustache-like Templates — http://timber.upstatement.com 

One of the first things I said to myself when I started learning Wordpress — there's gotta be a better way to write these templates. You can spread them across multiple tiny files, but that doesn't help with the fact that Wordpress templates are usually a soup of imperative code and HTML output, which can get unwieldy pretty fast either way. 

Timber lets you use your theme's `.php` files for code, and handle all markup in a declarative `.twig` template, which is peachy. Now I just have to figure out how my theme file should invite/coerce the user into installing _Timber_ along with it.

#### [Advanced Custom Fields](https://wordpress.org/plugins/advanced-custom-fields/)

> Customise WordPress with powerful, professional and intuitive fields.

ACF is mentioned in every second sentence on Wordpress development, and lots of themes and plugins mention they _play nicely_ with it. I still need to study it a bit to get what the fuss is about :-)

#### [Shortcake](https://wordpress.org/plugins/shortcode-ui/)

> Shortcake makes using WordPress shortcodes a piece of cake. 

This provides an UI for adding shortcodes, hasn't been updated for a while but looks good.

#### [W3 Total Cache](https://wordpress.org/plugins/w3-total-cache/)

> W3 Total Cache improves the user experience of your site by increasing website performance, reducing download times via features like content delivery network (CDN) integration.

This pretty much worked out of the box for minifying CSS/JS and for caching website content on the disk, making the website much faster.

#### [Post Types Order](https://wordpress.org/plugins/post-types-order/)

>  Post Order and custom Post Type Objects (custom post types) using a Drag and Drop Sortable JavaScript AJAX interface or default WordPress dashboard. 

Until Wordpress makes it easier to order posts of the same type (right now we're stuck with the Order input field on each post), this plugin helps order them with a simple drag & drop interface.

#### [Facebook Open Graph, Google+ and Twitter Card Tags](https://wordpress.org/plugins/wonderm00ns-simple-facebook-open-graph-tags/)

> Inserts Facebook Open Graph, Google+/Schema.org, Twitter and SEO Meta Tags into your WordPress Website for more efficient sharing results. 

Although people seem to be using [Yoast SEO](https://wordpress.org/plugins/wordpress-seo/) for this purpose, I appreciated that this plugin worked out of the box with decent results.

### Starter Themes

#### [\_s](https://github.com/Automattic/_s)

> Hi. I'm a starter theme called \_s, or underscores, if you like. I'm a theme meant for hacking so don't use me as a Parent Theme. Instead try turning me into the next, most awesome, WordPress theme out there. That's what I'm here for. — http://underscores.me/

I love this idea of ["A 1000-Hour Head Start"](http://themeshaper.com/2012/02/13/introducing-the-underscores-theme/).

#### [Timber Starter Theme](https://github.com/timber/starter-theme)

It's the `_s` of Timber-land. 

### Resources

#### [The WordPress Template Hierarchy](https://wphierarchy.com/)

This is a useful visualization of how [Wordpress decides which template to use](https://developer.wordpress.org/themes/basics/template-hierarchy) when displaying content on your site. Could work as a poster!
