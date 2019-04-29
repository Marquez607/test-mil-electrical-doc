.. _How-to-Write-Documentation:

==========================
How to Write Documentation
==========================

by :ref:`Matthew-Griessler`

Let's take a look at a really basic boiler plate restructure text page:

.. code-block:: rest
    :linenos:

    .. _File-Name-Without-Extension::

    ==========
    Page Title
    ==========

    Blah, blah, blah, blah.

# Make new .rst file with the name of the page you're adding. Place the document in the file structure of the project.
# The first line is a reference, they can be placed anywhere and used as link anchors, either from other documents or the same document. Putting one at the top of the page ensures the page can be linked to. `To make the page possible to link to, the first line should be a reference, it is written like this, note that it isn't indented: 

.. code-block:: rest

    .. _File-Name-Without-Extension::

# Next add a page title. Note that the equal signs above and below are the same length as the title. There should be a blank line between the reference line and the title lines. Like python, ReST is white spaced based.

.. code-block:: rest

    ==========
    Page Title
    ==========

# Add a blank line and then you can start writing text. This gets you a basic page. The whole thing should look like this:


# To compile your documentation navigate in your terminal of choice to the root folder of the documentation and run the command

.. code-block:: bash

    make html
    
This will compile the ReST into HTML, you can then open up the html file in the _build/html folder
    
# To fully write the page you'll probably need to know more ReST syntax. See other rst files in this project for examples or there are resources online. This is a `good, basic one <https://matplotlib.org/sampledoc/cheatsheet.html#making-links>`_.