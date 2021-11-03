# Structure

Let's say you want to add new pages to your site.

Open the file

	/domains/site.com/database/structures/default.ini

It has code like this inside:

	"index" : null,
	"news:content" : null,
	"contacts" : null

This file is an array in the **json** format, whose keys are the names of the site pages.

In most cases, the values ​​can be left blank.

If you want to create a section, specify a new array instead of the value. For example, like this:

	"index" : null,
	"news:content" : null,
	"new-group" : {
		"new-page" : null
	},
	"contacts" : null

All you have to do is go to the template folder and create a folder and file:

	/domains/site.com/templates/default/html/inner/new-group/
	/domains/site.com/templates/default/html/inner/new-group/new-page.php

In fact, structure is a powerful management tool. But for now, just treat it as an extra helper to keep you from cluttering your site.
