# Welcome to happy-kaseem Pages

A few months ago I started looking for a flexible way to create an online information system for an organisation. Personally I had been working with PHP and I am fairly familiar with HTML/CSS for now almost 20 years (this might tell you something on how old I am). I like tools which allow me to create the exact HTML I want (I know this is not what everybody is looking for).

I looked at every possible Content management system (CMS) and Framework I could find until I finally found ProcessWire. It was exactly what I was looking for:
- it is written in PHP
- it is simple and rather well documented (for users and developpers)
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
- in the database, the multi-language field data is stored for the default language in the default location whereas the other languages use separate data-n columns (for example column `data` for the default English, column `data1012` for German, column `data1020` for the next language etc.) are added. Note that the nubers depend on the installation. They are the page ids for the corresponding languages.
- at page rendering time, the fields are automatically filled either with the default language data or if available, the user language data for the language the user views the page for.

## An idea for a translation tool for PW

PW is great on how it handles multi-language fields using the built-in (core) Language Support module and the corresponding fields. I am trying to find a tool which supports the workflow of translating fields to different languages.

### Scenario

My idea is the following workflow.
- an editor writes one article in a specific source language
- translators translate the article to their target language
- the editor makes changes in the source language
- the translators see the changes and update the target language

For this we need a tool which does the following
- keep track of the changes to the article in every language
- show differences in the source language to show what needs to be translated in the target language (when in doubt, the translator can choose the version of the source which is used to show the differences).

This is a typical scenario (explicit example in English and German)

| Revision | Source (English)                              | Target (German)                                    |
| --------:| --------------------------------------------- | -------------------------------------------------- |
|      1   | This is the first version                     |                                                    |
|      2   | This is the corrected first version           |                                                    |
|      3   |                                               | Dies ist die korrigierte erste Version             |
|      4   | This is the extended, corrected first version |                                                    |
|      5   |                                               | Dies ist die erweiterte, korrigierte erste Version |

This revision history is typical when two different people work on the translation. Which would be the case in most situations.

So the tool should help the translator at any time to see what has changed in the source language since the last time he has made an update. So this would be after revision 2: All of the source, revision 2 needs to be translated (it is all new). And after Revision 4: Only the changes made to the source (comparing revision 2 and 4, in this case "extended, ") need to be translated and should be found by the tool based on the revision history since the tool knows that the translation in revision 3 is based on original revision 2. When the translator has some doubts, he can also compare the differences between source revision 1 and 4 (or 1 and 2 if that is helpful).

# How does PW handle page requests

What happens within PW between the request of a page and the actual answer of the server back to the web browser. 

## The Bootstrap Index File

In a default installation of PW, the http requests must be directed to index.php (the bootstrap index file) in the installation root folder. See PW documentation about [Directory Structure](https://processwire.com/docs/directories/) for details. In the default installation, the settings in the .htaccess file make sure that all requests to files in the installation are correctly handled. Here is an incomplete list (full details can be found in the root .htaccess file):
- requests to PW system files are denied. These are files like inc, info, info\.json, module, sh, sql.
- requests to PW system files in the site folder are denied (folders assets/cache, logs, backups, sessions, config. install, tmp)
- requests to PW configuration files are denied (/site/config.php)
- direct requests to PW template files are denied (/site/templates, /wire/templates-admin)
- requests to PW modules are denied (/site/module and /wire/module folders)

## What does the bootstrap index file do?

The bootstrap index PHP file does the following:
- Initialize $rootPath with PHP \_\_DIR\_\_, \_\_DIR\_\_ is a magic constant indicating the directory of the index.php file, the root directory ([see PHP manual for details](http://php.net/manual/en/language.constants.predefined.php)). The directory separator is forced to be '/' throughout PW.
- Initialize the composer autoloader as $composerAutoloader = $rootPath . '/vendor/autoload.php'. If the autoloader file exists, then load its PHP code now
- If the class "ProcessWire" has not been loaded yet, load it now from $rootPath/wire/core/ProcessWire.php
- Load the configuration data from the rootPath and initialize $config = ProcessWire::buildConfig($rootPath);





