---
title: What and why?
layout: post
---

# Welcome to happy-kaseem Pages

A few months ago I started looking for a flexible way to create an online information system for an organisation. Personally I had been working with PHP and I am fairly familiar with HTML/CSS for now almost 20 years (this might tell you something on how old I am).

I looked at every possible Content management system (CMS) and Framework I could find until I finally found ProcessWire. It was exactly what I was looking for:
- it is written in PHP
- it is simple and rather well documented
- it is flexible
- it has a multi-user access control system
- it is at its base already multi-language capable
- it is easily extensible
- and much more ...

For more information, go to their [website https://processwire.com](https://processwire.com)

The development for version 3.x is done on Github under [https://github.com/processwire/processwire](https://github.com/processwire/processwire) 

# Language support in processwire.com

Before we start explaining the language support of processwire.com it is helpful to understand something about the way processwire.com (or short PW) organises data.

Essentially everything in PW can be seen as a _page_. A _page_ contains information organised by _fields_ of a specific _type_. A standard web page would be defined by a template containing some standard fields named _title_, _body_, _summary_ etc. whereas each of these fields would by of type _text_. Other more sophisticated pages might call for fields of types like _URL_, _DateTime_ or more. 

The data is stored in separate tables for each field (named `field_`_fieldname_) with two default columns names `pages_id` and `data` for each field value per page (and more columns as the type stored might require).

This might sound like a very primitive concept. But if we really look at it and how PW has implemented this, it becomes very powerful, flexible and fast.

So how does now PW implement language support? Besides a number of tools to setup languages, select the user language and translate text, PW offers special multi-language _types_ for _fields_.

As much in the front-end as in the back-end, multi-language support is fully implemented and working.

## Things to get used to when working with languages

What took me as a developer the longest to understand are the following things:
- the default language has a special significance and can be seen as the language like it is used in a single-language site
- the user language is usually prefixed to the url (for example /home for the default language, /de/home for German etc.)
- in the database, the multi-language field data is stored for the default language in the default location whereas the other languages use separate data-n columns (for example column `data` for the default English, column `data1012` for German, column `data1020` for the next language etc.) are added.
- at page rendering time, the fields are automatically filled either with the default language data or if available, the user language data for the language the user views the page for.

