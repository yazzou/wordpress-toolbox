# An assorted bestiary of WordPress bugs, limitations, and gotchas — and their solution, whenever a satisfactory one exists

This are things I've learned the hard way.

## No nesting in <kbd>Appearance > Menus</kbd>

### Problem

You create your navigation menus in <kbd>Appearance > Menus</kbd>. You have many, many pages — or taxonomy terms, or custom posts — and they're nested; several of them may share the same title. [WordPress has a long-standing issue](https://core.trac.wordpress.org/ticket/18282) which causes the pages the pages you see in the Menu edit sidebar, under _View all_, not to maintain their nesting. It's very difficult, in this case, to figure out if you're choosing the correct item.

### Solution

Until WordPress resolves this issue, [there's a small plugin](https://core.trac.wordpress.org/attachment/ticket/18282/preserve-page-and-taxonomy-hierarchy.php) that fixes the issue by disabling the pagination on that list of items — this makes them nest properly.

## Can't work with images in Adobe RGB

### Problem

The built-in WordPress image editor (which gets invoked when you resize images) will read Adobe RGB images as sRGB and alter the colors. 

### Solution

There doesn't seem to be a way to fix this from WP or a plugin, so the only thing you can do here is make sure your images are sRGB.

## You can't nest pages of a type under pages of another type

### Problem

In order to _nest custom post types_, you need to declare them as hierarchical. Custom post types can only be nested inside _posts of the same type_ (bummer!)

### Solution

TBD.

## Hierarchical post types can't have all-numerical slugs

### Problem

If a post type is hierarchical, you will [not be able to set an all-numerical slug](http://wordpress.stackexchange.com/questions/186126/numeric-slug-on-child-post) for it, since it will conflict with checks for pagination.

### Workarounds

None I could figure out.

## Sorting things kinda sucks

### Problem

The order of pages (and other post types that support _page-attributes_) can only be changed from a numerical input in the _Edit page_ screen. This makes it almost useless in managing more than a handful of pages. 

### Solution

Plugins exist that allow you to rearrange the pages, but some of them don't support _hierarchical_ post types, or they charge for it. This is, however, a basic functionality that belongs to Wordpress Core.

## You can't use a shortcode inside itself

### Problem 

For example, if you were to build a layout system using shortcodes, you could not do this:

```
[row]
  [col]
    [row] ... [/row]
  [/col]
[/row]
```

For the reason, see the _Limitations_ section of [the Shortcode API](https://codex.wordpress.org/Shortcode_API). 

### Solution 

The workaround, as far as I can tell now, is to register the same shortcode under different names.

## `wpautop` will mess up your shortcodes

### Problem

[`wpautop`](https://developer.wordpress.org/reference/functions/wpautop/) is great; it will replace all double line-breaks with paragraph elements. It will also mess up the markup of your shortcodes. 

### Solution

[Some sort of workaround exists](https://wp-mix.com/wordpress-disable-extra-p-tags-shortcodes/), but it's not clear what the side-effects of the change are.

## Uploaded images with accented characters in their filename will not load on certain browsers

### Problem

If you upload an image that has diacritical marks in its filename, it will display as [broken in some browsers](https://core.trac.wordpress.org/ticket/22363).

__Note:__ Special characters in uploaded images can also cause problems when moving a WordPress site to a new host which may not support such filenames. (Happened to me!)

### Solution

## Don't forget to check the upload limit in your server's configuration

You don't want to launch with a (often default) 8mb file upload limit.

## Migration woes and trepidations

Some of the things I discovered when moving a WordPress site to a new host.

#### FTP is slow

A WordPress site tends to have a myriad of minuscule files, the transfer of which is not FTPs greatest strength. A much faster alternative is to zip it all up and extract it in place on the server, provided that the host comes with the ability to unzip the file (cPanel-powered hosts will probably have it).

#### Uploads with special characters in their filename can fail

For some reason, the old host supported filenames for the uploaded images that the new one did not. When importing to the new host, the unzipping process (see above) failed silently midway. A call to Tech Support revealed that the source of the problem was a © sign in a filename.

The solution was to rename the offending files, and make sure to replace any reference to it in the database. (I did a simple search and replace in the `database.sql` MySQL dump file before importing it).

#### WPML hit by amnesia? Check the table collations

Another head-scratcher was that on the new host WPML had deactivated and was inviting me to configure it from scratch. Turns out that a mismatch in the collations used for the various `icl_` MySQL tables was causing the issue. Adjusting all of them to the same `utf8_`-based collation fixed the problem.
