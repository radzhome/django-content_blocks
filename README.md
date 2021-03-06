Content Blocks
==============

This application provides a model and the necessary template tags for defining
editable content block areas in the templates. Without any need for initial
data, it can generate the blocks in the database when required. It also
provides an "Edit" link next to the blocks. If JavaScript is enabled and the
included JS class is loaded, these links enable AJAX-based, inline editing
of content right in the page. Otherwise, they become simple links to the admin
area of the selected block.


Installation
------------

**Dependencies**: django>=1.4x, markdown, django-linguo

Now compatible with django 1.7.x.

To install, download the source tarball, unpack and run ``pip install .``
This will add content_blocks to your python path as well as pull in the 
dependencies if they are not installed. 

To activate Content Blocks, include it in your ``INSTALLED_APPS`` list, under
your project's settings.py file. 

To render the content using markdown and show placeholder text when DEBUG is set
to True, the app depends on ``markup`` and ``webdesign`` applications,
included in ``django.contrib``.

    INSTALLED_APPS = (
        'django.contrib.markup',
        'django.contrib.webdesign',

        'linguo',
        'content_blocks',
    )

The second step is to include content_blocks.urls in your urls.py:

    urlpatterns = patterns('',
        url(_(r'^content-blocks/'), include('content_blocks.urls')),
    )

Finally, you'll need to **force admin into English mode or content blocks
will not work properly when editing in other languages.** Append the following 
to `MIDDLEWARE_CLASSES` in settings.py:

    MIDDLEWARE_CLASSES = (
        # ...
        'content_blocks.middleware.ForceEnglishInAdminMiddleware',
    )

After running ./manage.py syncdb, app will be ready to use.


Template Tags
-------------

Content Blocks can only be used through template tags. To define a block,
simply load the tag library and call it as such:

    {% load content_blocks_tags %}
    {% show_content_block "about" %}
    {% show_image_block "ceo" %}

``show_content_block`` only requires the unique identifier name of the block by
default. You can also provide three optional parameters: ``editable``,
``markup``, and ``amount``. If ``editable`` is set to "False", the edit link is
omitted. Similarly, if ``markup`` is set to "False", markdown is not applied to
the content. If ``amount`` is set to a number (or variable representing a
number) and there is no content for the block in the database, you will get
that many words of "lorem" text (if omitted or set to an empty string, you will
get the standard paragraph of approximately 70 words).

    {% show_content_block "about" "False" "False" 7 %}

To enable JavaScript functionality, you should import the JS enhancement app for
content blocks.

``show_image_block`` optionally takes a template name to override the
the template used to render the image block on a per image block basis:

    {% show_image_block "ceo" True "ceo_image_block.html" %}
