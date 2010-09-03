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

In this case we only had to override the ``get_queryset`` method to filter comments for the requesting user. This example might be a little contrived but it should hopefully illustrate the usefulness of class based views.


The ``panya`` app provides certain existing `Django generic views <http://docs.djangoproject.com/en/dev/ref/generic-views/>`_ *classified* in this manner, as well as additional class based views handy in general day to day Django work.

.. _class-based-generic-view-reference:

Class Reference
---------------

The following classes are contained in ``panya.generic.views``. 

.. _class-based-generic-view-reference-genericobjectlist:

GenericObjectList
+++++++++++++++++

.. _class-based-generic-view-reference-genericobjectdetail:

GenericObjectDetail
+++++++++++++++++++

.. _class-based-generic-view-reference-genericform:

GenericForm
+++++++++++



.. _class-based-generic-view-usage:

Usage
-----
