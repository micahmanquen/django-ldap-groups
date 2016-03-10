# Prerequisites #
## python-ldap ##
http://www.python-ldap.org/
Python-ldap has a number of dependencies itself, including OpenLDAP client
libraries, OpenSSL and optionally SASL.

## Django ##
http://djangoproject.com/
This app is known to work with v1.1, but should also work with 1.0, I don't
think I'm doing anything particularly odd that would preclude 1.0.

## An LDAP server ##
The app has been developed and tested against both Active
Directory and Novell eDirectory in live environments.  As per above, the
eDirectory authentication backend is reasonably generic, and will likely work
with most other LDAP server implementations with little or no modifications
necessary.  However, it has NOT been tested with any other servers, so there are
no guarantees.

## jQuery 1.3.2 ##
http://jquery.com/

## liveQuery ##
http://github.com/brandonaaron/livequery/tree/master
In addition, the admin change\_form.html uses and requires both jQuery and the
liveQuery jQuery plugin.  If you put your Javascripts elsewhere, you will need
to edit the script src attribute in the included change\_form.html to point to
your specific location.  My default is "/site\_media/js/", edit as needed.

# Installation #
## Basic Installation ##
To install django-ldap-groups, make it available on your PYTHONPATH, either via
"setup.py install" (with root privileges as needed), "pip install
django-ldap-groups", "easy\_install django-ldap-groups" or download, unpack and
link in site-packages.

## Django Settings ##

### LDAP Settings ###
You will also need to add several settings to your site's settings.py.  Included
in the app is a **settings.py.template** with all the necessary settings. The
NT4\_DOMAIN setting is used specifically with Active Directory, so please comment
it out if you are not using AD.  Likewise, there are two different sets of
SEARCH\_FIELDS settings, one of which is AD-specific, and the other is more
generic.  Depending on your LDAP schema, you may need to adjust this list _and_
the corresponding code that uses it.  If your LDAP server allows anonymous bind for searches, leave BIND\_USER and BIND\_PASSWORD active but set to ''.

The CERT\_FILE setting is necessary for using LDAPS, SSL-encrypted LDAP, and
should point to a local copy of the trusted root certificate for the certificate
authority that issued your LDAP server's SSL certificate.  To clarify that last
sentence for those of you whose heads just splorted, your LDAP server has an SSL
certificate.  Somewhere, sometime, that certificate was issued by a "Certificate
Authority".  For a commercial certificate, that CA was likely Verisign, Thawte,
or similar.  For an internal-only certificate, that CA was likely the very same
AD you are querying. Either way, the CA has a root certificate that is trusted
(by you, among others) to sign certificates.  A copy of the root certificate, in
base64 (PEM) encoded form, needs to reside somewhere your Django app can find
it, and the CERT\_FILE setting needs to point to it.  That is, if you want to use
LDAPS, which may or may not be a requirement for your shop.

### INSTALLED\_APPS ###
Add 'ldap\_groups' to your project's "INSTALLED\_APPS" settings, and run syncdb to
install the LDAPGroup model.

### AUTHENTICATION\_BACKENDS ###
Finally, in order to enable authentication, you need to add your choice of
'ldap\_groups.accounts.backends.ActiveDirectoryGroupMembershipSSLBackend' or
'ldap\_groups.accounts.backends.eDirectoryGroupMembershipSSLBackend' to a tuple
of AUTHENTICATION\_BACKENDS, as documented at
http://docs.djangoproject.com/en/dev/topics/auth/#specifying-authentication-backends
I strongly recommend your final string in AUTHENTICATION\_BACKENDS be
'django.contrib.auth.backends.ModelBackend', or your admin user will not be able
to log in to the application.