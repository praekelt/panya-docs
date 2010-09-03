.. _class-based-generic-views:

Class Based Generic Views
=========================

This document describes class based generic views provided by the ``panya`` app. Django itself is `moving towards class based generic views <http://code.djangoproject.com/ticket/6735>`_. 

**These views are a temporary solution that should only be used until such time as implemented by Django.**

.. contents:: Contents
    :depth: 5

.. _class-based-generic-view-background:

A Little Background
-------------------

A Django view is a **callable** that takes a request object and returns a response object. Therefore it is completely acceptable for a view to be an object which has a __call__ method defined on its class.

Class based views are particularly useful in that they allow for greater reuse of code and cleaner implementations. For instance consider the following class based view listing all comments and allowing for users to post comments. Some of the particulars have been omitted for brevity::

    class CommentListing(object):

        def handle_GET(self, request, context):
            """
            Handle GET request and return a response.
            """

        def handle_POST(self, request, context):
            """
            Handle POST request and return a response.
            """

        def get_context(self, request):
            return RequestContext(request, {'queryset': self.get_queryset(request)})

        def get_queryset(self, request):
            return Comment.objects.all() 

        def __call__(self, request):
            context = self.get_context()
            if request.method == 'POST':
                return self.handle_POST(request, context)
            if request.method == 'GET':
                return self.handle_GET(request, context)

For example sake lets say you now want to create a view listing comments and allowing users to post comments, limited to a particular user. A new view can now be derived from the ``CommentListing`` view with minimal code changes/additions, i.e.::
    
    class UserCommentListing(CommentListing):

        def get_queryset(self, request):
            return Comment.objects.filter(user=request.user)

In this case we only had to override the ``get_queryset`` method to filter comments for the requesting user. This example might be somewhat contrived but it should hopefully illustrate the usefulness of class based views.


The ``panya`` app provides certain existing `Django generic views <http://docs.djangoproject.com/en/dev/ref/generic-views/>`_ *classified* in this manner, as well as additional class based views handy in general day to day Django work.

.. _class-based-generic-view-reference:

Class Based Generic View Reference
----------------------------------

The following class based generic views are contained in ``panya.generic.views``. 

.. _class-based-generic-view-reference-genericobjectlist:

GenericObjectList
+++++++++++++++++

A class based wrapper around `django.views.generic.list_detail.object_list <http://docs.djangoproject.com/en/dev/ref/generic-views/#django-views-generic-list-detail-object-list>`_. Arguments can still be provided as per normal via a URL pattern, or alternatively via overriden :ref:`class methods <class-based-generic-view-reference-genericobject-list-methods>`.

.. _class-based-generic-view-reference-genericobject-list-methods:

methods
~~~~~~~

.. _class-based-generic-view-reference-genericobject-list-methods-get_allow_empty:

get_allow_empty(request, \*args, \*\*kwargs)
********************************************
Returns a boolean specifying whether to display the page if no objects are available. If this is ``False`` and no objects are available, the view will raise a 404 instead of displaying an empty page. 

Default: ``True``

.. _class-based-generic-view-reference-genericobject-list-methods-get_context_processors:

get_context_processors(request, \*args, \*\*kwargs)
***************************************************
Returns a list of template-context processors to apply to the view's template.

Default: ``None``

.. _class-based-generic-view-reference-genericobject-list-methods-get_extra_context:

get_extra_context(request, \*args, \*\*kwargs)
********************************************
Returns a dictionary of values to add to the template context. If a value in the dictionary is callable, the generic view will call it just before rendering the template.

Default: ``None``

.. _class-based-generic-view-reference-genericobject-list-methods-get_mimetype:

get_mimetype(request, \*args, \*\*kwargs)
*****************************************
Returns the MIME type to use for the resulting document. Defaults to the value of the `DEFAULT_CONTENT_TYPE setting <http://docs.djangoproject.com/en/dev/ref/settings/#default-content-type>`_.

Default: ``None``

.. _class-based-generic-view-reference-genericobject-list-methods-get_page:

get_page(request, \*args, \*\*kwargs)
********************************************
Returns the current page number, as an integer, or the string 'last'. This is 1-based.

Default: ``None``

.. _class-based-generic-view-reference-genericobject-list-methods-get_paginate_by:

get_paginate_by(request, \*args, \*\*kwargs)
********************************************
Returns an integer specifying how many objects should be displayed per page. If this is given, the view will paginate objects with paginate_by objects per page. The view will expect either a page query string parameter (via GET) or a page variable specified in the URLconf.

Default: ``None``

.. _class-based-generic-view-reference-genericobject-list-methods-get_queryset:

get_queryset(request, \*args, \*\*kwargs)
*****************************************
Returns a QuerySet that represents the objects.

Default: ``None``

.. _class-based-generic-view-reference-genericobject-list-methods-get_template_loader:

get_template_loader(request, \*args, \*\*kwargs)
**********************************************
Returns template loader to use when loading the template. If a template loader isn't specified the view will use the ``django.template.loader`` template loader by default.

Default: ``None``

.. _class-based-generic-view-reference-genericobject-list-methods-get_template_name:

get_template_name(request, \*args, \*\*kwargs)
**********************************************
Returns the full name of a template to use in rendering the page. If template_name isn't specified the view will use the template ``<app_label>/<model_name>_list.html`` by default.

Default: ``None``

.. _class-based-generic-view-reference-genericobject-list-methods-get_template_object_name:

get_template_object_name(request, \*args, \*\*kwargs)
*****************************************************
Returns the name of the template variable to use in the template context. The view will append ``'_list'`` to the value of this parameter in determining the variable's name.

Default: ``'object'``


.. _class-based-generic-view-reference-genericobjectdetail:

GenericObjectDetail
+++++++++++++++++++

A class based wrapper around `django.views.generic.list_detail.object_detail <http://docs.djangoproject.com/en/dev/ref/generic-views/#django-views-generic-list-detail-object-detail>`_.

.. _class-based-generic-view-reference-genericform:

GenericForm
+++++++++++



.. _class-based-generic-view-usage:

Usage
-----
