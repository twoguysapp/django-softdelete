## What is different in this fork

Normally each record would have a 'deleted' checkbox next to it that would
allow any staff member to (un)delete an object quickly and easily by
(un)checking the box.

A side effect of this is that all objects, soft-deleted or otherwise are shown
in the admin by default.

For my uses I would rather not show (soft) deleted objects, so in this fork they
are not shown. The way to undelete soft-deleted records is therefore handled via
the changesets.

This pattern allows me (the admin) to quickly undo any erroneous deletes made
by staff.

# django-softdelete

Soft delete for Django ORM, with support for undelete.

Can be tested directly with the following command:

    django-admin.py test softdelete --settings="softdelete.settings"

Inspired by http://codespatter.com/2009/07/01/django-model-manager-soft-delete-how-to-customize-admin/

This project mostly exists to be able to provide undelete of soft-deleted objects, along with proper
undeletion of related objects.

Requirements:

 * Django 1.3
 * django.contrib.contenttypes

## Installation

 1. Add `'softdelete'` to your `INSTALLED_APPS`.
 2. Have your models extend from `softdelete.models.SoftDeleteObject`. If you have existing models,
use Django South to create a schema migration to add the `deleted_at` Datetime field to each
of your models.
 3. Extend from or use directly the `softdelete.admin.SoftDeleteObjectAdmin` model admin
class in your `admin.py` files.

There are simple templates files in templates/.  You will need to add Django's
egg loader to use the templates as is:

    TEMPLATE_LOADERS = (
    ...
        'django.template.loaders.eggs.Loader',
    )

Add the project 'softdelete' to your INSTALLED_APPS for 
through-the-web undelete support.

    INSTALLED_APPS = (
    ...
        'django.contrib.contenttypes',
        'softdelete',
    )

Central to the ability to undelete a soft-deleted model is the concept of changesets.  When you
soft-delete an object, any objects referencing it via a ForeignKey, ManyToManyField, or OneToOneField will
also be soft-deleted.  This mimics the traditional CASCADE behavior of an SQL DELETE.

When the soft-delete is performed, the system makes a ChangeSet object which tracks all affected objects of
this delete request.  Later, when an undelete is requested, this ChangeSet is referenced to do a cascading 
undelete.

If you are undeleting an object that was part of a ChangeSet, that entire ChangeSet is undeleted.  

Once undeleted, the ChangeSet object is removed from the underlying database with a regular ("hard") delete.
