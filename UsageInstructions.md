# Usage #
Once you have installed django-ldap-groups, and run syncdb, log in to the admin
app as a superuser, go to Auth -> Groups and create a group or two, and assign
permissions to the group.  Next, go to Ldap\_groups -> Ldap group and create an
LDAPGroup mapping.  The easy way to do this is to type in the "Search for LDAP
CN" field a fragment of the OU you want to assign to a particular Django group,
and click the Search LDAP button.  Assuming you've configured everything
properly, a list of OUs that match the text you typed will be displayed below.
Click the proper OU, and the OU's DN should populate the Org Unit textarea.
Next, choose which Django groups to assign, and set the flags for "make\_staff"
and "make\_superuser" to meet your needs.  Finally, save the model.

At this point, any user who logs in to your application and authenticates via
LDAP, if the user belongs to the OU you chose in the mapping above, that user
will automatically be created as a django.contrib.auth.models.User in the Django
database, and they will automatically be assigned to whichever groups you
specified above.  In addition, if you set make\_staff or make\_superuser to True,
that user will automatically be set to staff, superuser, or both.

One note:  I designed the logic so that if **any** LDAPGroup that a user's OU
memberships match has make\_staff or make\_superuser set to True, then the user
will be marked is\_staff or is\_superuser.  The site administrator (that would be
you, by the way) is responsible for making sure that their users fall into the
proper groups and inherit the proper permissions.

Err, another note:  This application does not handle per-object permissions, but
it should not interfere with e.g. Jannis Leidel's excellent django-authority
(http://bitbucket.org/jezdez/django-authority), and the two should work together
quite well.