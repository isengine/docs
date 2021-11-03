# Installation

## At first

You must have hosting or virtual server.

Suppose your site will be accessible at

	http://site.com

And his project is located in the folder

	/domains/site.com/

It also assumes that you have the latest version of **composer** installed.

## Installing via composer

You can read in detail about different installation methods, but for now just run **composer**, go to the folder

	/domains/

enter a command like this:

	composer create-project --stability dev --prefer-dist isengine/isengine site.com

and press **Enter**.

Go to the root folder of your project

	/domains/site.com/

Find the folder there

	public

And rename it to the one your site starts with. For example, it could be one of the following:

* public_html
* www
* docs

## Install in browser

Now you need to open your site in a browser.

If everything is configured correctly, you will see the installation window.

Here it is enough to press the **Install** button and wait for the end.

After finishing press the button **go to the site**.

Now go to the root folder of your project again

	/domains/site.com/

Find the folder there

	install

It needs to be removed.

## That's all

Not really.

You don't need an empty, bare site with which you can't do anything.

By the way, the site is already up and running. Well, how much time has passed?

Now let's see how things get set up here.