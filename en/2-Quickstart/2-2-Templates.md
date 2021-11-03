# Templates

In folder

	/domains/site.com/templates/

templates are stored. The default template is **default**. Go to it.

Here you will see folders with styles and scripts, but we need a folder

	/domains/site.com/templates/default/html/

It contains the file

	template.php

This is the main template file from which all downloads come from.

In folder

	/domains/site.com/templates/default/html/inner/

there are files corresponding to the pages of your site. And subfolders correspond to sections.

The main file of the section is called

	index.php

All main sections and pages, such as **news**, **contacts** or **about us**, are already there.

In folder

	/domains/site.com/templates/default/html/blocks/

there are blocks. These site page elements can be used in different places in the template and even in another template.

Let's open the file now

	/domains/site.com/templates/default/html/template.php

You see this code in it:

	$view -> get('blocks') -> launch('item:opening', 'default');

This line opens the **item: opening** block from the **default** template. Namely, this file:

	/domains/site.com/templates/default/html/blocks/item/opening.php

Thus, you can assemble any page from blocks.