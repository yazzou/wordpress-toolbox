# Wordpress Toolbox

This is a compendium of Wordpress tips, tricks, tools and recipes I'm gathering while I'm learning how to use it. Also check out [the Wiki](https://github.com/danburzo/wordpress-toolbox/wiki), which acts as a purgatory for half-formed ideas before they make it here.

## Table of Contents

* [Plugins](#plugins)
* [Starter themes](#starter-themes)
* [Recipes](#recipes)
* [Resources](#resources)

## Plugins

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

### Page Builders

Page builders let you build the content of your page or post as a series of sections in a visual way. What I'm looking for is a _light_ page builder that can work with _controlled_ sections (i.e. allow _some_ flexibility, but only use components I've defined). Here's a list of plugins that seem to be popular:

Name | WP Plugin | GitHub repo | Notes
---- | --------- | ----------- | -----
[SiteOrigin Page Builder](https://siteorigin.com/page-builder/) | [siteorigin-panels](https://wordpress.org/plugins/siteorigin-panels/) | [siteorigin/siteorigin-panels](https://github.com/siteorigin/siteorigin-panels) | Entirely free and comes with a [widgets bundle](https://wordpress.org/plugins/so-widgets-bundle/) ([GitHub](https://github.com/siteorigin/so-widgets-bundle))
[Elementor](https://elementor.com/) | [elementor](https://wordpress.org/plugins/elementor/) | [pojome/elementor](https://github.com/pojome/elementor) | Free and paid versions. I assume the GitHub repo and WordPress listing refer to the free version.
[Beaver Builder](https://www.wpbeaverbuilder.com/) | [beaver-builder-lite-version](https://wordpress.org/plugins/beaver-builder-lite-version/) | N/A | Free and paid versions.
[Visual Composer](https://vc.wpbakery.com/) | N/A | N/A | Paid-only. #1 paid plugin on CodeCanyon.
[Advanced Custom Fields](https://www.advancedcustomfields.com/) | [advanced-custom-fields](https://wordpress.org/plugins/advanced-custom-fields/) | N/A | Free and paid versions. The paid versions contains a _Flexible content_ field that can be used as a page builder.
[Live Composer](https://livecomposerplugin.com/) | [live-composer-page-builder](https://wordpress.org/plugins/live-composer-page-builder/) | [live-composer/live-composer-page-builder](https://github.com/live-composer/live-composer-page-builder) | Free.

For an easy solution with excellent flexibility, you can also use custom Shortcodes (the output of which you can manage very nicely with Timber / Twig). The downside is with migrating content to a different theme that does not know how to interpret them, but is it really better if the page builder just spews out very specific HTML markup?

(Also, I need to check out Widgets -- do they help?)

__Further reading:__ [WordPress Page builder plugins: a critical review](https://pippinsplugins.com/wordpress-page-builder-plugins-critical-review/) is a thoughtful & thorough article.

## Starter Themes

#### [\_s](https://github.com/Automattic/_s)

> Hi. I'm a starter theme called \_s, or underscores, if you like. I'm a theme meant for hacking so don't use me as a Parent Theme. Instead try turning me into the next, most awesome, WordPress theme out there. That's what I'm here for. — http://underscores.me/

I love this idea of ["A 1000-Hour Head Start"](http://themeshaper.com/2012/02/13/introducing-the-underscores-theme/).

#### [Timber Starter Theme](https://github.com/timber/starter-theme)

It's the `_s` of Timber-driven themes and it has all the things to get you hacking right away. 

## Recipes

#### Changing the number of posts per page

In the example below, we're overriding the number of posts to show on an archive page for all custom post types (CPTs). The default is 10. This goes into `functions.php`:

```php
add_action( 'pre_get_posts', 'configure_posts_per_page' );

function configure_posts_per_page($query) {
    // Don't alter queries in the admin interface
    // and don't alter any query that's not the main one
    if (is_admin() || !$query->is_main_query()) {
        return;
    } 
    // For custom post type based archives, override the number of posts per page
    if ($query->is_post_type_archive()) {
        $query->set('posts_per_archive_page', 12);
    }
}
```

A couple of things to be mindful about: we only alter queries that happen on the user-facing interface (we check for `is_admin()` and return early), and only on the main query (the check for `$query->is_main_query()`), because the [`pre_get_posts` hook](https://codex.wordpress.org/Plugin_API/Action_Reference/pre_get_posts) gets triggered for _all_ the queries happening for the request and not doing these checks might have unintended consequences, like breaking the pagination in the admin interface, or hindering performance.

#### Showing only top-level posts for a hierarchical custom post type archive page

If you've created a custom post type that allows nesting — that is, your posts can have a parent (of the same post type) — and you want to only display the top-level posts on its archive page, you can use the [`pre_get_posts` hook](https://codex.wordpress.org/Plugin_API/Action_Reference/pre_get_posts) to filter out posts that have a parent:

```php
function configure_get_posts($query) {

    // Don't alter queries in the admin interface
    // and don't alter any query that's not the main one
    if (is_admin() || !$query->is_main_query()) {
        return;
    } 

    // For custom post type based archives
    if ($query->is_post_type_archive()) {
        // Only show top-level posts
        if ($query->query_vars['post_parent'] == false) {
            $query->set('post_parent', 0);
        }
    }
}
```

__Note:__ All the caveats from the previous tip apply here as well.

#### Laying out posts in a grid using Twig

If you're using Timber to develop your theme, you can use the [`batch` filter](http://twig.sensiolabs.org/doc/filters/batch.html) in Twig to elegantly lay out posts in a grid. In the example below, we're writing markup for a three-column grid layout:

```twig
{% for row in posts | batch(3) %}
  <div class='row'>
  {% for post in row %}
    <div class='col'>
      ...your post here...
    </div>
  {% endfor %}
  </div>
{% endfor %}
```

#### Using different languages for the website's front-end and the administration dashboard

Previously, you'd do this via hacks, but [starting from Wordpress 4.7](https://wordpress.org/news/2016/12/vaughan/) you can set the language for the dashboard independently from the website language by going to your _Profile_ settings in the administration interface.

## Resources

#### [The WordPress Template Hierarchy](https://wphierarchy.com/)

This is a useful visualization of how [Wordpress decides which template to use](https://developer.wordpress.org/themes/basics/template-hierarchy) when displaying content on your site. Could work as a poster!

## Wordpress limitations about which I found the hard way

#### You can't nest pages of a type under pages of another type

Bummeriño :(

#### You can't use a shortcode inside itself

For example, if you were to build a layout system using shortcodes, you could not do this:

```
[row]
  [col]
    [row] ... [/row]
  [/col]
[/row]
```

The workaround, as far as I can tell now, is to register the same shortcode under different names.

#### `wpautop` will mess up your shortcodes

[`wpautop`](https://developer.wordpress.org/reference/functions/wpautop/) is great; it will replace all double line-breaks with paragraph elements. It will also mess up the markup of your shortcodes. [Some sort of workaround exists](https://wp-mix.com/wordpress-disable-extra-p-tags-shortcodes/), but it's not clear what the side-effects of the change are.

## Miscellaneous tips

#### How to find which theme and plugins a WordPress website is using

If you're curious about how other websites use WordPress, the page source can give a few hints. You can search for `wp-content/themes` and `wp-content/plugins` to see what's included. (Of course, this only works for WP websites that don't do CSS/JS bundling, or serve assets from CDNs). Here's a short snippet that you can paste into the browser Console to inspect a website for themes/plugins:

```js
function _match(str, regex) {
	var matches = [];
	str.replace(regex, function() {
	    matches.push(Array.prototype.slice.call(arguments));
	});
	return matches;
}

function _unique(item, idx, arr) {
	return arr.indexOf(item) === idx;
}

function find_themes(str) {
	return _match(str, /wp\-content\/themes\/([^\/]+)/g)
		.map(function(item) {
			return item[1];
		})
		.filter(_unique);
}

function find_plugins(str) {
	return _match(str, /wp\-content\/plugins\/([^\/]+)/g).map(function(item) {
		return item[1];
	})
	.filter(_unique);
}

function inspect_wp() {
	var html = document.documentElement.innerHTML;
	console.info('Found themes:');
	console.table(find_themes(html));
	console.info('Found plugins:');
	console.table(find_plugins(html));
}

inspect_wp();
```

Once you've identified the theme name, the path `/wp-content/themes/<theme-name>/style.css` will usually give you more information in the CSS header (this is also used by WordPress to display info about the theme).
