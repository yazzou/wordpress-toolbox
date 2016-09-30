# Wordpress Toolbox

Wordpress tips/tricks/tools I'm gathering along the way while learning how to use it.

## Plugins

### [Query Monitor](https://wordpress.org/plugins/query-monitor/)

>  View debugging and performance information on database queries, hooks, conditionals, HTTP requests, redirects and more. 

I found this to be very useful for debugging a variety of things such as which hooks have been triggered, which theme files are used on the current page, what conditionals match it, et cetera.

### [Timber](https://wordpress.org/plugins/timber-library/)

> Helps you create themes faster with sustainable code. With Timber, you write HTML using Mustache-like Templates — http://timber.upstatement.com 

One of the first things I said to myself when I started learning Wordpress — there's gotta be a better way to write these templates. You can spread them across multiple tiny files, but that doesn't help with the fact that Wordpress templates are usually a soup of imperative code and HTML output, which can get unwieldy pretty fast either way. 

Timber lets you use your theme's `.php` files for code, and handle all markup in a declarative `.twig` template, which is peachy. Now I just have to figure out how my theme file should invite/coerce the user into installing _Timber_ along with it.
