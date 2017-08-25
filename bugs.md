# An assorted bestiary of WordPress bugs, limitations, and gotchas — and their solution, whenever a satisfactory one exists

This are things I've learned the hard way.

## No nesting in <kbd>Appearance > Menus</kbd>

### Problem

You create your navigation menus in <kbd>Appearance > Menus</kbd>. You have many, many pages — or taxonomy terms, or custom posts — and they're nested; several of them may share the same title. [WordPress has a long-standing issue](https://core.trac.wordpress.org/ticket/18282) which causes the pages the pages you see in the Menu edit sidebar, under _View all_, not to maintain their nesting. It's very difficult, in this case, to figure out if you're choosing the correct item.

### Solution

Until WordPress resolves this issue, [there's a small plugin](https://core.trac.wordpress.org/attachment/ticket/18282/preserve-page-and-taxonomy-hierarchy.php) that fixes the issue by disabling the pagination on that list of items — this makes them nest properly.
