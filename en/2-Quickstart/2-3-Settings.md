# Settings

All system settings are in the folder

	/domains/site.com/database/

This folder is a local database.

Let's open the file

	/domains/site.com/database/templates/default.ini

This is a **json** file, only here you can wrap lines, and here you can also use comments.

Look in the section

	webmaster

Here you can set api keys and counter number for google and yandex. They will automatically be included in the site code.

Let's go down to the section

	assets

He is responsible for including styles and scripts in the template.

It consists of three subsections:

	head
	body
	ending

Each subsection is responsible for a specific place in the code.

Head - in the **head** tag of the site.

Body - after the opening **body** tag.

Ending - at the very end, before the **body** closing tag.

Look at this line:

	cdn:https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js

It includes the **jquery** library located on the CDN server.

Here is a line

	font:Roboto:100,400,700,900:cyrillic

Connects Google Fonts. In this case, you just need to specify the name of the font:

	font:Open Sans

This line

	css:template

includes file

	/domains/site.com/templates/default/css/template.css

This file is first copied to the site's public folder.

By analogy, scripts and other styles are included.

If you need to use subfolders, specify the path separated by a colon:

	css:subfolder:template

The less and scss styles are available directly from the core, and you can connect them as easily as the regular ones, either separately or together:

	css:template
	less:template
	scss:template

All three styles will be placed in different folders and will not interfere with each other.