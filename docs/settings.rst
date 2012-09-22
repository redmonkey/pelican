Settings
########

Pelican is configurable thanks to a configuration file you can pass to
the command line::

    $ pelican -s path/to/your/settingsfile.py path

Settings are configured in the form of a Python module (a file). You can see an
example by looking at `/samples/pelican.conf.py
<https://github.com/getpelican/pelican/raw/master/samples/pelican.conf.py>`_

All the setting identifiers must be set in all-caps, otherwise they will not be
processed. Setting values that are numbers (5, 20, etc.), booleans (True,
False, None, etc.), dictionaries, or tuples should *not* be enclosed in
quotation marks. All other values (i.e., strings) *must* be enclosed in
quotation marks.

Unless otherwise specified, settings that refer to paths can be either absolute or relative to the
configuration file.

The settings you define in the configuration file will be passed to the
templates, which allows you to use your settings to add site-wide content.

Here is a list of settings for Pelican:

Basic settings
==============

=====================================================================   =====================================================================
Setting name (default value)                                            What does it do?
=====================================================================   =====================================================================
`AUTHOR`                                                                Default author (put your name)
`DATE_FORMATS` (``{}``)                                                 If you do manage multiple languages, you can
                                                                        set the date formatting here. See "Date format and locales"
                                                                        section below for details.
`DEFAULT_CATEGORY` (``'misc'``)                                         The default category to fall back on.
`DEFAULT_DATE_FORMAT` (``'%a %d %B %Y'``)                               The default date format you want to use.
`DISPLAY_PAGES_ON_MENU` (``True``)                                      Whether to display pages on the menu of the
                                                                        template. Templates may or not honor this
                                                                        setting.
`DEFAULT_DATE` (``fs``)                                                 The default date you want to use.
                                                                        If 'fs', Pelican will use the file system
                                                                        timestamp information (mtime) if it can't get
                                                                        date information from the metadata.
                                                                        If tuple object, it will instead generate the
                                                                        default datetime object by passing the tuple to
                                                                        the datetime.datetime constructor.
`JINJA_EXTENSIONS` (``[]``)                                             A list of any Jinja2 extensions you want to use.
`DELETE_OUTPUT_DIRECTORY` (``False``)                                   Delete the content of the output directory before
                                                                        generating new files.
`LOCALE` (''[#]_)                                                       Change the locale. A list of locales can be provided
                                                                        here or a single string representing one locale.
                                                                        When providing a list, all the locales will be tried
                                                                        until one works.
`MARKUP` (``('rst', 'md')``)                                            A list of available markup languages you want
                                                                        to use. For the moment, the only available values
                                                                        are `rst` and `md`.
`MD_EXTENSIONS` (``['codehilite','extra']``)                            A list of the extensions that the Markdown processor
                                                                        will use. Refer to the extensions chapter in the
                                                                        Python-Markdown documentation for a complete list of
                                                                        supported extensions.
`OUTPUT_PATH` (``'output/'``)                                           Where to output the generated files.
`PATH` (``None``)                                                       Path to content directory to be processed by Pelican.
`PAGE_DIR` (``'pages'``)                                                Directory to look at for pages, relative to `PATH`.
`PAGE_EXCLUDES` (``()``)                                                A list of directories to exclude when looking for pages.
`ARTICLE_DIR` (``''``)                                                  Directory to look at for articles, relative to `PATH`.
`ARTICLE_EXCLUDES`: (``('pages',)``)                                    A list of directories to exclude when looking for articles.
`PDF_GENERATOR` (``False``)                                             Set to True if you want to have PDF versions
                                                                        of your documents. You will need to install
                                                                        `rst2pdf`.
`RELATIVE_URLS` (``True``)                                              Defines whether Pelican should use document-relative URLs or
                                                                        not. If set to ``False``, Pelican will use the SITEURL
                                                                        setting to construct absolute URLs.
`PLUGINS` (``[]``)                                                      The list of plugins to load. See :ref:`plugins`.
`SITENAME` (``'A Pelican Blog'``)                                       Your site name
`SITEURL`                                                               Base URL of your website. Not defined by default,
                                                                        so it is best to specify your SITEURL; if you do not, feeds
                                                                        will not be generated with properly-formed URLs. You should
                                                                        include ``http://`` and your domain, with no trailing
                                                                        slash at the end. Example: ``SITEURL = 'http://mydomain.com'``
`STATIC_PATHS` (``['images']``)                                         The static paths you want to have accessible
                                                                        on the output path "static". By default,
                                                                        Pelican will copy the 'images' folder to the
                                                                        output folder.
`TIMEZONE`                                                              The timezone used in the date information, to
                                                                        generate Atom and RSS feeds. See the "timezone"
                                                                        section below for more info.
`TYPOGRIFY` (``False``)                                                 If set to True, several typographical improvements will be
                                                                        incorporated into the generated HTML via the `Typogrify
                                                                        <http://static.mintchaos.com/projects/typogrify/>`_
                                                                        library, which can be installed via: ``pip install typogrify``
`LESS_GENERATOR` (``FALSE``)                                            Set to True or complete path to `lessc` (if not
                                                                        found in system PATH) to enable compiling less
                                                                        css files. Requires installation of `less css`_.
`DIRECT_TEMPLATES` (``('index', 'tags', 'categories', 'archives')``)    List of templates that are used directly to render
                                                                        content. Typically direct templates are used to generate
                                                                        index pages for collections of content e.g. tags and
                                                                        category index pages.
`PAGINATED_DIRECT_TEMPLATES` (``('index',)``)                           Provides the direct templates that should be paginated.
`SUMMARY_MAX_LENGTH` (``50``)                                           When creating a short summary of an article, this will
                                                                        be the default length in words of the text created.
                                                                        This only applies if your content does not otherwise
                                                                        specify a summary. Setting to None will cause the summary
                                                                        to be a copy of the original content.

=====================================================================   =====================================================================

.. [#] Default is the system locale.

.. _less css: http://lesscss.org/


URL settings
------------

The first thing to understand is that there are currently two supported methods
for URL formation: *relative* and *absolute*. Document-relative URLs are useful
when testing locally, and absolute URLs are reliable and most useful when
publishing. One method of supporting both is to have one Pelican configuration
file for local development and another for publishing. To see an example of this
type of setup, use the ``pelican-quickstart`` script as described at the top of
the :doc:`Getting Started<getting_started>` page, which will produce two separate
configuration files for local development and publishing, respectively.

You can customize the URLs and locations where files will be saved. The URLs and
SAVE_AS variables use Python's format strings. These variables allow you to place
your articles in a location such as '{slug}/index.html' and link to them as
'{slug}' for clean URLs. These settings give you the flexibility to place your
articles and pages anywhere you want.

.. note::
    If you specify a datetime directive, it will be substituted using the
    input files' date metadata attribute. If the date is not specified for a
    particular file, Pelican will rely on the file's mtime timestamp.

Check the Python datetime documentation at http://bit.ly/cNcJUC for more
information.

Also, you can use other file metadata attributes as well:

* slug
* date
* lang
* author
* category

Example usage:

* ARTICLE_URL = 'posts/{date:%Y}/{date:%b}/{date:%d}/{slug}/'
* ARTICLE_SAVE_AS = 'posts/{date:%Y}/{date:%b}/{date:%d}/{slug}/index.html'

This would save your articles in something like '/posts/2011/Aug/07/sample-post/index.html',
and the URL to this would be '/posts/2011/Aug/07/sample-post/'.

================================================    =====================================================
Setting name (default value)                        what does it do?
================================================    =====================================================
`ARTICLE_URL` ('{slug}.html')                       The URL to refer to an ARTICLE.
`ARTICLE_SAVE_AS` ('{slug}.html')                   The place where we will save an article.
`ARTICLE_LANG_URL` ('{slug}-{lang}.html')           The URL to refer to an ARTICLE which doesn't use the
                                                    default language.
`ARTICLE_LANG_SAVE_AS` ('{slug}-{lang}.html'        The place where we will save an article which
                                                    doesn't use the default language.
`PAGE_URL` ('pages/{slug}.html')                    The URL we will use to link to a page.
`PAGE_SAVE_AS` ('pages/{slug}.html')                The location we will save the page.
`PAGE_LANG_URL` ('pages/{slug}-{lang}.html')        The URL we will use to link to a page which doesn't
                                                    use the default language.
`PAGE_LANG_SAVE_AS` ('pages/{slug}-{lang}.html')    The location we will save the page which doesn't
                                                    use the default language.
`AUTHOR_URL` ('author/{name}.html')                 The URL to use for an author.
`AUTHOR_SAVE_AS` ('author/{name}.html')             The location to save an author.
`CATEGORY_URL` ('category/{name}.html')             The URL to use for a category.
`CATEGORY_SAVE_AS` ('category/{name}.html')         The location to save a category.
`TAG_URL` ('tag/{name}.html')                       The URL to use for a tag.
`TAG_SAVE_AS` ('tag/{name}.html')                   The location to save the tag page.
`<DIRECT_TEMPLATE_NAME>_SAVE_AS`                    The location to save content generated from direct
                                                    templates. Where <DIRECT_TEMPLATE_NAME> is the
                                                    upper case template name.
================================================    =====================================================

.. note::

    When any of `*_SAVE_AS` is set to False, files will not be created.

Timezone
--------

If no timezone is defined, UTC is assumed. This means that the generated Atom
and RSS feeds will contain incorrect date information if your locale is not UTC.

Pelican issues a warning in case this setting is not defined, as it was not
mandatory in previous versions.

Have a look at `the wikipedia page`_ to get a list of valid timezone values.

.. _the wikipedia page: http://en.wikipedia.org/wiki/List_of_tz_database_time_zones


Date format and locale
----------------------

If no DATE_FORMAT is set, fall back to DEFAULT_DATE_FORMAT. If you need to
maintain multiple languages with different date formats, you can set this dict
using language name (``lang`` in your posts) as key. Regarding available format
codes, see `strftime document of python`_ :

.. parsed-literal::

    DATE_FORMAT = {
        'en': '%a, %d %b %Y',
        'jp': '%Y-%m-%d(%a)',
    }

You can set locale to further control date format:

.. parsed-literal::

    LOCALE = ('usa', 'jpn',  # On Windows
        'en_US', 'ja_JP'     # On Unix/Linux
        )

Also, it is possible to set different locale settings for each language. If you
put (locale, format) tuples in the dict, this will override the LOCALE setting
above:

.. parsed-literal::
    # On Unix/Linux
    DATE_FORMAT = {
        'en': ('en_US','%a, %d %b %Y'),
        'jp': ('ja_JP','%Y-%m-%d(%a)'),
    }

    # On Windows
    DATE_FORMAT = {
        'en': ('usa','%a, %d %b %Y'),
        'jp': ('jpn','%Y-%m-%d(%a)'),
    }

This is a list of available `locales on Windows`_ . On Unix/Linux, usually you
can get a list of available locales via the ``locale -a`` command; see manpage
`locale(1)`_ for more information.


.. _strftime document of python: http://docs.python.org/library/datetime.html#strftime-strptime-behavior

.. _locales on Windows: http://msdn.microsoft.com/en-us/library/cdax410z%28VS.71%29.aspx

.. _locale(1): http://linux.die.net/man/1/locale

Feed settings
=============

By default, Pelican uses Atom feeds. However, it is also possible to use RSS
feeds if you prefer.

Pelican generates category feeds as well as feeds for all your articles. It does
not generate feeds for tags by default, but it is possible to do so using
the ``TAG_FEED_ATOM`` and ``TAG_FEED_RSS`` settings:

================================================    =====================================================
Setting name (default value)                        What does it do?
================================================    =====================================================
`FEED_DOMAIN` (``None``, i.e. base URL is "/")      The domain prepended to feed URLs. Since feed URLs
                                                    should always be absolute, it is highly recommended
                                                    to define this (e.g., "http://feeds.example.com"). If
                                                    you have already explicitly defined SITEURL (see
                                                    above) and want to use the same domain for your
                                                    feeds, you can just set:  `FEED_DOMAIN = SITEURL`
`FEED_ATOM` (``'feeds/all.atom.xml'``)              Relative URL to output the Atom feed.
`FEED_RSS` (``None``, i.e. no RSS)                  Relative URL to output the RSS feed.
`CATEGORY_FEED_ATOM` ('feeds/%s.atom.xml'[2]_)      Where to put the category Atom feeds.
`CATEGORY_FEED_RSS` (``None``, i.e. no RSS)         Where to put the category RSS feeds.
`TAG_FEED_ATOM` (``None``, i.e. no tag feed)        Relative URL to output the tag Atom feed. It should
                                                    be defined using a "%s" match in the tag name.
`TAG_FEED_RSS` (``None``, ie no RSS tag feed)       Relative URL to output the tag RSS feed
`FEED_MAX_ITEMS`                                    Maximum number of items allowed in a feed. Feed item
                                                    quantity is unrestricted by default.
================================================    =====================================================

If you don't want to generate some of these feeds, set ``None`` to the
variables above. If you don't want to generate any feeds set both ``FEED_ATOM``
and ``FEED_RSS`` to none.

.. [2] %s is the name of the category.

FeedBurner
----------

If you want to use FeedBurner for your feed, you will likely need to decide
upon a unique identifier. For example, if your site were called "Thyme" and
hosted on the www.example.com domain, you might use "thymefeeds" as your
unique identifier, which we'll use throughout this section for illustrative
purposes. In your Pelican settings, set the `FEED_ATOM` attribute to
"thymefeeds/main.xml" to create an Atom feed with an original address of
`http://www.example.com/thymefeeds/main.xml`. Set the `FEED_DOMAIN` attribute
to `http://feeds.feedburner.com`, or `http://feeds.example.com` if you are
using a CNAME on your own domain (i.e., FeedBurner's "MyBrand" feature).

There are two fields to configure in the `FeedBurner
<http://feedburner.google.com>`_ interface: "Original Feed" and "Feed
Address". In this example, the "Original Feed" would be
`http://www.example.com/thymefeeds/main.xml` and the "Feed Address" suffix
would be `thymefeeds/main.xml`.

Pagination
==========

The default behaviour of Pelican is to list all the article titles along
with a short description on the index page. While it works pretty well
for small-to-medium blogs, for sites with large quantity of articles it would
be convenient to have a way to paginate the list.

You can use the following settings to configure the pagination.

================================================    =====================================================
Setting name (default value)                        What does it do?
================================================    =====================================================
`DEFAULT_ORPHANS` (0)                               The minimum number of articles allowed on the
                                                    last page. Use this when you don't want to
                                                    have a last page with very few articles.
`DEFAULT_PAGINATION` (False)                        The maximum number of articles to include on a
                                                    page, not including orphans. False to disable
                                                    pagination.
================================================    =====================================================

Tag cloud
=========

If you want to generate a tag cloud with all your tags, you can do so using the
following settings.

================================================    =====================================================
Setting name (default value)                        What does it do?
================================================    =====================================================
`TAG_CLOUD_STEPS` (4)                               Count of different font sizes in the tag
                                                    cloud.
`TAG_CLOUD_MAX_ITEMS` (100)                         Maximum number of tags in the cloud.
================================================    =====================================================

The default theme does not support tag clouds, but it is pretty easy to add::

    <ul>
        {% for tag in tag_cloud %}
            <li class="tag-{{ tag.1 }}"><a href="/tag/{{ tag.0 }}/">{{ tag.0 }}</a></li>
        {% endfor %}
    </ul>

You should then also define a CSS style with the appropriate classes (tag-0 to tag-N, where
N matches `TAG_CLOUD_STEPS` -1).

Translations
============

Pelican offers a way to translate articles. See the Getting Started section for
more information.

================================================    =====================================================
Setting name (default value)                        What does it do?
================================================    =====================================================
`DEFAULT_LANG` (``'en'``)                           The default language to use.
`TRANSLATION_FEED` ('feeds/all-%s.atom.xml'[3]_)    Where to put the feed for translations.
================================================    =====================================================

.. [3] %s is the language

Ordering content
=================

================================================    =====================================================
Setting name (default value)                        What does it do?
================================================    =====================================================
`NEWEST_FIRST_ARCHIVES` (``True``)                  Order archives by newest first by date. (False:
                                                    orders by date with older articles first.)
`REVERSE_CATEGORY_ORDER` (``False``)                Reverse the category order. (True: lists by reverse
                                                    alphabetical order; default lists alphabetically.)
================================================    =====================================================

Themes
======

Creating Pelican themes is addressed in a dedicated section (see :ref:`theming-pelican`).
However, here are the settings that are related to themes.

================================================    =====================================================
Setting name (default value)                        What does it do?
================================================    =====================================================
`THEME`                                             Theme to use to produce the output. Can be a relative
                                                    or absolute path to a theme folder, or the name of a
                                                    default theme or a theme installed via
                                                    ``pelican-themes`` (see below).
`THEME_STATIC_PATHS` (``['static']``)               Static theme paths you want to copy. Default
                                                    value is `static`, but if your theme has
                                                    other static paths, you can put them here.
`CSS_FILE` (``'main.css'``)                         Specify the CSS file you want to load.
`WEBASSETS` (``False``)                             Asset management with `webassets` (see below)
================================================    =====================================================


By default, two themes are available. You can specify them using the `THEME` setting or by passing the
``-t`` option to the ``pelican`` command:

* notmyidea
* simple (a synonym for "plain text" :)

There are a number of other themes available at http://github.com/getpelican/pelican-themes.
Pelican comes with :doc:`pelican-themes`, a small script for managing themes.

You can define your own theme, either by starting from scratch or by duplicating
and modifying a pre-existing theme. Here is :doc:`a guide on how to create your theme <themes>`.

Following are example ways to specify your preferred theme::

    # Specify name of a built-in theme
    THEME = "notmyidea"
    # Specify name of a theme installed via the pelican-themes tool
    THEME = "chunk"
    # Specify a customized theme, via path relative to the settings file
    THEME = "themes/mycustomtheme"
    # Specify a customized theme, via absolute path
    THEME = "~/projects/mysite/themes/mycustomtheme"

The built-in `notmyidea` theme can make good use of the following settings. Feel
free to use them in your themes as well.

=======================   =======================================================
Setting name              What does it do ?
=======================   =======================================================
`DISQUS_SITENAME`         Pelican can handle Disqus comments. Specify the
                          Disqus sitename identifier here.
`GITHUB_URL`              Your GitHub URL (if you have one). It will then
                          use this information to create a GitHub ribbon.
`GOOGLE_ANALYTICS`        'UA-XXXX-YYYY' to activate Google Analytics.
`GOSQUARED_SITENAME`      'XXX-YYYYYY-X' to activate GoSquared.
`MENUITEMS`               A list of tuples (Title, URL) for additional menu
                          items to appear at the beginning of the main menu.
`PIWIK_URL`               URL to your Piwik server - without 'http://' at the
                          beginning.
`PIWIK_SSL_URL`           If the SSL-URL differs from the normal Piwik-URL
                          you have to include this setting too. (optional)
`PIWIK_SITE_ID`           ID for the monitored website. You can find the ID
                          in the Piwik admin interface > settings > websites.
`LINKS`                   A list of tuples (Title, URL) for links to appear on
                          the header.
`SOCIAL`                  A list of tuples (Title, URL) to appear in the
                          "social" section.
`TWITTER_USERNAME`        Allows for adding a button to articles to encourage
                          others to tweet about them. Add your Twitter username
                          if you want this button to appear.
=======================   =======================================================

In addition, you can use the "wide" version of the `notmyidea` theme by
adding the following to your configuration::

    CSS_FILE = "wide.css"

Asset management
----------------

The `WEBASSETS` setting allows you to use the `webassets`_ module to manage
assets such as CSS and JS files. The module must first be installed::

    pip install webassets

The `webassets` module allows you to perform a number of useful asset management
functions, including:

* CSS minifier (`cssmin`, `yuicompressor`, ...)
* CSS compiler (`less`, `sass`, ...)
* JS minifier (`uglifyjs`, `yuicompressor`, `closure`, ...)

Others filters include gzip compression, integration of images in CSS via data
URIs, and more. `webassets` can also append a version identifier to your asset
URL to convince browsers to download new versions of your assets when you use
far-future expires headers. Please refer to the `webassets documentation`_ for
more information.

When using with Pelican, `webassets` is configured to process assets in the
``OUTPUT_PATH/theme`` directory. You can use `webassets` in your templates by
including one or more template tags. For example...

.. code-block:: jinja

    {% assets filters="cssmin", output="css/style.min.css", "css/inuit.css", "css/pygment-monokai.css", "css/main.css" %}
        <link rel="stylesheet" href="{{ ASSET_URL }}">
    {% endassets %}

... will produce a minified css file with a version identifier:

.. code-block:: html

    <link href="http://{SITEURL}/theme/css/style.min.css?b3a7c807" rel="stylesheet">

These filters can be combined. Here is an example that uses the SASS compiler
and minifies the output:

.. code-block:: jinja

    {% assets filters="sass,cssmin", output="css/style.min.css", "css/style.scss" %}
        <link rel="stylesheet" href="{{ ASSET_URL }}">
    {% endassets %}

Another example for Javascript:

.. code-block:: jinja

    {% assets filters="uglifyjs,gzip", output="js/packed.js", "js/jquery.js", "js/base.js", "js/widgets.js" %}
        <script src="{{ ASSET_URL }}"></script>
    {% endassets %}

The above will produce a minified and gzipped JS file:

.. code-block:: html

    <script src="http://{SITEURL}/theme/js/packed.js?00703b9d"></script>

Pelican's debug mode is propagated to `webassets` to disable asset packaging
and instead work with the uncompressed assets. However, this also means that
the LESS and SASS files are not compiled. This should be fixed in a future
version of `webassets` (cf. the related `bug report
<https://github.com/getpelican/pelican/issues/481>`_).

.. _webassets: https://github.com/miracle2k/webassets
.. _webassets documentation: http://webassets.readthedocs.org/en/latest/builtin_filters.html

Example settings
================

.. literalinclude:: ../samples/pelican.conf.py
    :language: python
