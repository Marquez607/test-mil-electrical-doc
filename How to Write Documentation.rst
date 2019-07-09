.. _How-to-Write-Documentation:

**************************
How to Write Documentation
**************************

by :ref:`Matthew-Griessler`

I've written this documentation without too much detail, if there's anything that is unclear or doesn't work. Basically if you have to duckduckgo anything message me on slack and I'll flesh this documentation out. I want to make it easy for everyone to get started.

System Setup
============

To write documentation you will need several items:

#. An account on github.com
#. git installed on your computer. If you are unfamiliar with git, download github desktop. It includes everything you need and a GUI interface. And if you ever want to switch to the command line git interface, it is also an option. https://desktop.github.com/
#. Python, install from https://www.python.org/downloads/. Use the latest stable 3.X.X version. If you follow the link and click the gold button you'll install the correct version. Make sure you check the box to add python to the PATH.
#. Sphinx, after installing python, from the command prompt run ``pip install -U Sphinx``

How to Contribute
=================

Now how do you start contributing?

You'll need to fork the electrical documentation repository. It is currently hosted at https://github.com/mgriessler/test-mil-electrical-doc. Github hosts a forking guide here: https://help.github.com/en/articles/fork-a-repo.

Now you can edit pages/add new pages. There's a basic ReStructuredText explanation in the bottom section of this page. I'm a big fan of Notepad++ https://notepad-plus-plus.org/. Especially with the Solarized color theme.

Once you've made your edits test them out with Sphinx.

#. Open the command prompt (I'm a fan of Win+R->cmd->Enter)
#. Navigate to where you cloned your fork of test-mil-electrical-doc
#. Run the command ``make html``

If everything works you'll see ``Running Sphinx vX.X.X``, a bunch of stuff, ending with ``build succeeded``, possibly with N warnings. Any warnings will be in red, if any of them are in the files you edited or if their are errors, address them. The last text in the build tells you where the HTML files are located, which is in the _build\html directory. Navigate there in file explorer and open the file you edited/added, to make sure it looks the way you want. 

If you're happy with how things turned out, push your changes to your repo on github.com and then submit a pull request and I'll merge your changes in! https://help.github.com/en/articles/creating-a-pull-request-from-a-fork

ReStructuredText Primer
=======================

Sphinx uses the markup language RestructedText. There are a number of advantages of RestructuredText over something like markdown. It's not too difficult to figure out if you only know markdown, or if you don't know either.

Honestly the easiest thing to do is find a page that has what you want and look at the syntax. Common syntax will be documented here as I notice patterns.

Notice the numbering below keeps resetting itself, feel free to fix this problem and remove this notice.

Let's take a look at a really basic boiler plate restructure text page:

.. code-block:: rest
    :linenos:

    .. _File-Name-Without-Extension::

    ==========
    Page Title
    ==========

    Blah, blah, blah, blah.

#. Make new .rst file with the name of the page you're adding. Place the document in the file structure of the project.
#. The first line is a reference, they can be placed anywhere and used as link anchors, either from other documents or the same document. Putting one at the top of the page ensures the page can be linked to. To make the page possible to link to, the first line should be a reference, it is written like this, note that it isn't indented: 

.. code-block:: rest

    .. _File-Name-Without-Extension::

#. Next add a page title. Note that the equal signs above and below are the same length as the title. There should be a blank line between the reference line and the title lines. Like python, ReST is white spaced based.

.. code-block:: rest

    ==========
    Page Title
    ==========

#. Add a blank line and then you can start writing text. This gets you a basic page.
    
#. To fully write the page you'll probably need to know more ReST syntax. See other rst files in this project for examples or there are resources online. This is a `good, basic one <https://matplotlib.org/sampledoc/cheatsheet.html#making-links>`_.