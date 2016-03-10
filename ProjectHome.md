# What it is #
Django-ldap-groups is a reusable application for the Django web framework. This project is **unmaintained**, please feel free to fork. I have not had a need to authenticate against LDAP for a few years now.

# Current Version #
Please use version **0.1.3**, prior versions had incorrect setup.py files that would fail to install the template.

# What it does #
The django-ldap-groups app provides two pluggable authentication
backends for Django that allow web app users to authenticate against
an LDAP server, one backend for Active Directory and another for Novell
eDirectory.  I suspect the eDirectory backend will work with minimal
modifications for most other LDAP servers.

In addition, the app provides a model that allows site
administrators to map LDAP organizational units (OUs) to Django groups.  These
maps provide a way to assign a user authenticated via LDAP to particular Django
groups in order to acquire the permissions set on these groups.

With the combination of the authentication backends, which both authenticate
users and create them if they do not already exist in the Django database, and
the LDAP OU mappings, django-ldap-groups provides a single application that
gives site administrators the means to handle both authentication and
authorization based on the structures already in place in LDAP.

# Where it comes from #
This app was inspired by a number of snippets on Django Snippets, though
primarily on http://www.djangosnippets.org/snippets/901/ by "mary".  Her AD
authN backend was what I modified to what you see here.  From there, I expanded
to allow eDirectory to work with it, and added the LDAPGroup mapping and admin
modifications.