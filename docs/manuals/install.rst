Installation
============

The *Online interactive Risk Assessment tool (OiRA)* is an adaptation of 
`Euphorie`_, which is an add-on product for the `Plone`_ content management
system.

The requirements for running an OiRA site are:

* Plone 4.2.
* an SQL database
* two separate virtual hosts

Plone instalation
-----------------
To install OiRA you will first need to `download`_ and install Plone.
OiRA requires Plone 4.2.  After installing Plone you can install
OiRA. To do this you will need to edit the ``base.cfg`` file (part of the
`buildout <http://www.buildout.org>`_ configuration) of your
Plone installation. This file is normally located in the ``zinstance``
directory if your Plone install.  

Look for the *find-links* parameter under the ``buildout`` section
and add the syslab.com egg server::

  [buildout]
  find-links =
    ...
	http://products.syslab.com/products/simple


Look for an *eggs* line under the ``instance`` section and add OiRA there::

  [instance]
  ...
  eggs =
      osha.oira
      psycopg2 # for using postgresql as your database backend


This will instruct Plone to install the OiRA software. Next you will
need to add some *zcml* entries to load the necessary configuration as well::

  [instance]
  ...
  zcml =
      osha.oira
      osha.oira-overrides
      euphorie.deployment-meta
      euphorie.deployment
      euphorie.deployment-overrides

After making these two changes you must (re)run buildout and restart your
Zope/Plone instance. Navigate to your ``zinstance`` directory and type::

    $ bin/buildout
    $ bin/instance restart

A new *Euphorie website* option should now appear in the list of add-on products
in your Plone control panel. Installing this will setup Euphorie in your site.

For more information on installing add-on products in your Plone site please
see the article `installing an add-on product`_ in the Plone knowledge base.


Configuration
-------------

Euphorie uses `z3c.appconfig <http://pypi.python.org/pypi/z3c.appconfig>`_ to
handle application configuration. All values are stored in the ``euphorie``
section. For example::

  [euphorie]
  client=http://oira.example.com

The available options are:

. table:: configuration options

   +--------------------------+-------------------------------------------+
   | options                  | Description                               |
   +==========================+===========================================+
   | ``client``               | URL for the client (see also              |
   |                          | :ref:`Virtual hosting <virtualhosting>`)  |
   +--------------------------+-------------------------------------------+
   | ``terms-and-conditions`` | Boolean flag indicating it the client     |
   |                          | must ask users to accept the terms        |
   |                          | and conditions of the site.               |
   +--------------------------+-------------------------------------------+

SQL database
------------

Euphorie uses a SQL database to store information for users of the client. Any
SQL database supported by SQLALchemy_ should work. If you have selected a
database you will need to configure it in ``buildout.cfg``. For example if
you use postgres you will first need to make sure that the psycopg_ driver
is installed by adding it to the ``eggs`` of the ``instance`` section::

  [instance]
  ...
  eggs =
      osha.oira 
      psycopg2

next you need to configure the database connection information. This requires
a somewhat verbose statement in the ``instance`` section of ``buildout.cfg``::

  [instance]
  ...
  zcml-additional =
     <configure xmlns="http://namespaces.zope.org/zope"
                xmlns:db="http://namespaces.zope.org/db">
         <include package="z3c.saconfig" file="meta.zcml" />
         <db:engine name="session" url="postgres:///euphorie" />
         <db:session engine="session" />
     </configure>

Make sure The ``url`` parameter is correct for the database you want to use.
It uses the standard SQLAlchemy connection URI format.

To setup the database you must run buildout and run the database initialisation
command::

    $ bin/buildout
    $ bin/instance initdb

.. note::

   You need Zope 2.12.12 or later to be able to use the ``initdb`` command. For
   earlier Zope versions you need to specify the path for the
   :py:mod:`euphorie.deployment.commands.xmlimport` module on the command line.


.. _virtualhosting:

Virtual hosting
---------------

Euphorie requires two separate virtual hosts: one host for the client, and one
for CMS tasks. It is common to use ``oira.example.com`` as hostname for the
client and ``admin.oira.example.com`` as hostname for the CMS. The standard
method for configuring virtual hosting for Plone sites apply here as well. The
Plone website has instructions for `configuring Plone with Apache`_ and
`configuring Plone with Enfold Proxy on Windows`_. Here is an example Apache
configuration::

  <VirtualHost *:80>
      ServerName admin.oira.example.com
      ProxyPass / http://localhost:8080/VirtualHostBase/http/admin.oira.example.com:80/Plone/VirtualHostRoot/

      # Prevent access to the client using the administrative site.
      <Location /client>
          order allow, deny
          deny form all
      </Location>
  </VirtualHost>

  <VirtualHost *:80>
      ServerName oira.example.com
      ProxyPass / http://localhost:8080/VirtualHostBase/http/admin.oira.example.com:80/Plone/client/VirtualHostRoot/
  </VirtualHost>


You will also need to configure the URL for the client in the ``euphorie.ini`` file::

  [euphorie]
  client=http://oira.example.com


.. _Euphorie: pypi.python.org/pypi/Euphorie
.. _Plone: http://plone.org/
.. _download: http://plone.org/download
.. _installing an add-on product: http://plone.org/documentation/kb/third-party-products/installing
.. _SQLAlchemy: http://sqlalchemy.org/
.. _psycopg: http://initd.org/psycopg/
.. _configuring Plone with Apache: http://plone.org/documentation/kb/plone-with-apache
.. _configuring Plone with Enfold Proxy on Windows: http://plone.org/documentation/kb/managing-your-plone-sites-in-windows-with-enfold-proxy

