Entity Relationship Diagramming with Skipper
============================================

In the world of Object Relational Mapping one tool stands alone.  `Skipper <https://skipper18.com>`_ is a visual
diagramming tool which exports to Doctrine metadata.

Often when a project is new the metadata for the application is handled solely through annotations.  This is not
a good solution.  Projects usually start this way because the full suite of tools available to ORM developer is
not realized.  Skipper is one of these overlooked tools.

Using Skipper every relationship in your ORM can be visualized and all the metadata can be handled without touching
the code.  Take for instance you want to add a field to an entity:  Using Skipper you will visually add the new field,
export the XML metadata (note:  always us XML metadata), run ``orm:generate-entities`` then run ``orm:schema-tool:update --dump-sql``
to get the SQL you must add to your `migrations <http://docs.doctrine-project.org/projects/doctrine-migrations/en/latest/toc.html>`_.

The visual diagram Skipper provides you enables your entire team from CEO to JS Programmer to understand the data model of the application.
To compare if you used annotations any user interested in the schema would need to read each file and interpret the annotions and
memorize each annotation.  So, when your CEO can speak the diction of your application because he can see the field names and relationships
in the ORM your entire organization will benefit.  And when a JS Programmer can see the relationships visually and map those to the
HAL response they recieve from the API everyone will benefit.

It is strongly recommended to migrate your ORM metadata into Skipper before creating a Doctrine in Apigility API.

.. role:: raw-html(raw)
   :format: html

.. note::
  Authored by `API Skeletons <https://apiskeletons.com>`_.  All rights reserved.


:raw-html:`<script async src="https://www.googletagmanager.com/gtag/js?id=UA-64198835-2"></script><script>window.dataLayer = window.dataLayer || [];function gtag(){dataLayer.push(arguments);}gtag('js', new Date());gtag('config', 'UA-64198835-2');</script>`
