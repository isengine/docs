# Basic concepts and terms

## isEngine

isEngine is the general name for the system. Derived from the words "is" (this) and "engine" (engine). Consonant with "easy" (simple, easy).

We deliberately use just such a spelling - together, with a lowercase "i" and an uppercase "E". The uppercase "I" (i) is similar to the lowercase "l" (L) in some fonts. Using a capital letter at the beginning can create misread. The capital "E" inside the word separates the word "Engine" from the word "is".

In your language, such a spelling may be illiterate, but from a programming point of view, such a spelling corresponds to the "Camel case" style and is not an error.

## System

By the words "isEngine" and "system" we mean the entire software package. In fact, it is, of course, made up of many components, including third party libraries and custom code.

However, when we talk about it as a single whole, we use the words "system" or "isEngine".

We do not mean the end product - website or web application.

## System architecture

If we consider the architecture of the system, then we see a tower with the following levels:

* website or web app
* system
* framework
* programming language

The finished product is the top of the tower, the goal, what everything was created for. However, it uses the system as a framework.

At the center of the system is the kernel, which controls the entire system. This core, in turn, is built using a set of tools - a framework that has already been developed for a specific programming language.

Here we want to note that we perceive the framework as a set of tools for creating the engine of the system, and not as the engine itself.

## Another framework

Yes, this is a lyrical digression.

In fact, when designing the system from scratch, there was a thought to use the existing framework. However, the existing frameworks did not meet the requirements for the format of presentation, exchange and processing of data.

Thus, isEngine has its own micro-framework, which you can also take as a basis, or use in your project as a library.

## Keyword 'system'

Throughout the rest of the tutorial, we will often refer to "system data" and "system objects".

The fact is that at the very beginning it is necessary to clearly distinguish a certain category of data from the whole set of their types. The word "system" is a good fit to define this category.

First of all, these are the data that correspond to the format accepted in isEngine. Just like objects.

## File Formats

There are only two allowed file formats:

* **.ini** - for storing data in **json** format
* **.php** - for all other system files

All classes, system files and template pages use the **php** extension.

All data files have the **ini** extension.

System security means that an attacker cannot gain direct access to read and modify these files. Therefore, you can set restrictions on access to these files.

## Format of paths for files and folders

IsEngine has two rules for specifying absolutely all paths.

The folder path format **always** must end with a closing slash.

The format of paths for folders and files **should never** begin with a slash.

Also, do not forget to use the slash **[/]** in **URL**, and the **DS** constant in the paths of the server file system!

Example for folders:

	folder       # wrong
	/folder      # wrong
	/folder/     # wrong
	folder/      # correct

Example for files:

	folder/      # correct
	/folder.php  # wrong
	/folder.php/ # wrong
	folder.php/  # wrong

These rules may not match the rules in the file system, but following them helps to avoid path compilation errors.

Example for PHP:

    $folder = 'folder/';
    $path = DR . $folder . 'file.php';
    echo $path;

Result:

    ...directory_root/base/path/folder/file.php

Example for URL:

    $page = 'page.html';
    $folder = 'folder/';
    $path = 'https://site.com/' . $folder . $page . '?one=a';
    echo $path;

Result:

    https://site.com/folder/page.html?one=a

**Attention!** Example of an error:

    $page = '/page.html';
    $folder = '/folder/';
    $path = 'https://site.com/' . $folder . '/' . $page . '?one=a';
    echo $path;

Result:

    https://site.com//folder///page.html?one=a

As you can see, violation of these rules leads to sad consequences, the constant need to monitor variables and clean paths from multiple slashes.

## URL format

Any **URL** must conform to the accepted standard.

Let's look at the following example:

    http://site.com/path/to/page.html?one=1&two=2#name

This **URL** consists of the following parts:

* http              # scheme, request scheme
* site.com          # host, host address (site, domain)
* path/to/page.html # path, path
* one=1&two=2       # query, query parameters
* name              # fragment, fragment

Unfortunately, the fragments of the **URL request** are not recognized on the server side. So when parsing a **URl request**, the fragment field will always be empty.

Each **URL** represents a request to some resource. First of all, the **URL** identifies this resource. The **scheme** (scheme), **address** (host) and **path** (path) of the request are used here.

But **actions** or **methods** are not a resource. To perform any **action**, we need to contact the server. In other words, make up a request. **Parameters** (query) make up the parameters of this query.

The request can consist of **headers** - purely service information and **body** - in fact, the content of this request.

The server responds to the request in the same way. The **body** of the response includes the content of the page (for the site), or other information provided by the server. These can be service codes or an error message.

The **HTTP** protocol defines the following **methods**:

* GET - gets full data (**body** and **headers**)
* HEAD - receives only **headers** (service information)
* POST - sends data to the server
* PUT - sends a request to create a new resource on the server, or to replace an existing one
* PATCH - sends a request to change a fragment of a resource
* DELETE - sends a request to delete a resource
* CONNECT - establishes a permanent bi-directional connection (tunnel) with the server
* OPTIONS - sends a request to get connection parameters
* TRACE - sends a request, and receives it back for comparison (for example, what information intermediate servers add or change in the request)

**Methods** **GET**, **HEAD** and **OPTIONS** are considered safe, because by the established standard, calling these methods does not change the state of the resource (data on the server). The rest are not. Therefore, the server must be sure to check them.

By default, **methods** **GET** and **HEAD** are cached, the rest are not. Therefore, when using the **method** **POST**, you can be sure that the request will be executed. In the case of **GET**, you can get a response from the cache, so the request directly to the server may not reach the server.

Some servers may not accept certain **methods** at all.
