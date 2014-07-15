.. _security:

Security
========

This is not intended to be a comprehensive security document, but is a high
level introduction or refresher for some security best practices resources.
Properly securing a CAS server means understanding your specific security
requirements and any unique aspects of your setup. It is important to
understand each component of your specific stack and ensure it is configured
properly.

MamaCAS Configuration
---------------------

Open vs. Closed
~~~~~~~~~~~~~~~

By default, MamaCAS operates in an "open" mode that allows any service to
make use of its functionality. It is recommended that production CAS servers
are configured as "closed" by specifying allowed services with
``MAMA_CAS_VALID_SERVICES``. Services not matching one of these patterns
will not be able to validate tickets, nor will MamaCAS redirect to
unspecified URLs.

Django Configuration
--------------------

Sessions
~~~~~~~~

MamaCAS relies on standard Django sessions to govern single sign-on sessions.
There are two primary Django session settings that should be considered:

   `SESSION_COOKIE_AGE`_
      It is recommended this be set shorter than the default of two weeks.
      This setting controls the duration of single sign-on sessions as well
      as the duration of proxy-granting tickets.

   `SESSION_EXPIRE_AT_BROWSER_CLOSE`_
      This should be set to ``True`` to conform to the CAS specification.
      Note that some browsers can be configured to retain cookies across
      browser restarts, even for cookies set to be removed on browser close.

For more information on Django sessions, read the `session documentation`_.

Best Practices
~~~~~~~~~~~~~~

The Django documentation includes some great `security best practices`_ that
are important to review when setting up any Django site. Some of them do not
apply to a dedicated CAS server, but most of them are directly applicable.

Web Server
----------

Securing a web server is a vast topic completely outside the scope of this
guide and the details depend on the specific server in use. However, some
universal configuration options ought to be considered.

SSL
~~~

It goes without saying that a login server should require `SSL`_. Without it,
login credentials and CAS tickets are exposed to anyone sniffing the
connection. Additionally, all services making use of CAS should be
communicating via SSL as well.

HSTS
~~~~

`HTTP Strict Transport Security`_ (HSTS) tells browsers that the site should
only use HTTPS and not HTTP. When a browser encounters this header, it will
automatically use HTTPS on future visits. This prevents some man-in-the-middle
attacks caused by browsers initially accessing the page as HTTP.

X-Frame-Options
~~~~~~~~~~~~~~~

The `X-Frame-Options`_ header indicates whether a page may appear inside a
``<frame>``, ``<iframe>`` or ``<object>`` to help prevent clickjacking
attacks. If the site legitimately appears within one of these elements,
specified domains may be whitelisted.

.. _SESSION_COOKIE_AGE: https://docs.djangoproject.com/en/dev/ref/settings/#std:setting-SESSION_COOKIE_AGE
.. _SESSION_EXPIRE_AT_BROWSER_CLOSE: https://docs.djangoproject.com/en/dev/ref/settings/#std:setting-SESSION_EXPIRE_AT_BROWSER_CLOSE
.. _session documentation: https://docs.djangoproject.com/en/dev/topics/http/sessions/
.. _security best practices: https://docs.djangoproject.com/en/dev/topics/security/
.. _SSL: https://developer.mozilla.org/en-US/docs/Introduction_to_SSL
.. _HTTP Strict Transport Security: https://developer.mozilla.org/en-US/docs/Web/Security/HTTP_strict_transport_security
.. _X-Frame-Options: https://developer.mozilla.org/en-US/docs/Web/HTTP/X-Frame-Options