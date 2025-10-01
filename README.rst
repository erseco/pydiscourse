===========
pydiscourse
===========

.. image:: https://img.shields.io/pypi/v/pydiscourse?color=blue
   :alt: PyPI
   :target: https://pypi.org/project/pydiscourse/

.. image:: https://github.com/pydiscourse/pydiscourse/actions/workflows/ci.yml/badge.svg
    :alt: CI
    :target: https://github.com/pydiscourse/pydiscourse/actions

.. image:: https://img.shields.io/badge/Check%20out%20the-Docs-blue.svg
    :alt: Check out the Docs
    :target: https://discourse.readthedocs.io/en/latest/


A Python client for the Discourse API.

This project started as a fork of the original Tindie version to add fixes and
new functionality. It provides a thin, well-documented wrapper around the
official Discourse HTTP API.

Goals
=====

* Exceptional documentation
* Support all supported Python versions
* Provide functional parity with the Discourse API, for the currently supported
  version of Discourse (something of a moving target)

The order here is important. The Discourse API is itself poorly documented so
the level of documentation in the Python client is critical.

Installation
============

Install from PyPI:

::

    pip install pydiscourse

Quickstart
==========

Create a client connection to a Discourse server:

.. code:: python

  from pydiscourse import DiscourseClient

  client = DiscourseClient(
      'http://example.com',
      api_username='username',
      api_key='areallylongstringfromdiscourse',
  )

Get info about a user:

.. code:: python

  user = client.user('eviltrout')
  print(user)

  user_topics = client.topics_by('johnsmith')
  print(user_topics)

Create a new user:

.. code:: python

  user = client.create_user(
      'The Black Knight',
      'blacknight',
      'knight@python.org',
      'justafleshwound',
  )

Implement SSO for Discourse with your Python server:

.. code:: python

  @login_required
  def discourse_sso_view(request):
      payload = request.GET.get('sso')
      signature = request.GET.get('sig')
      nonce = sso_validate(payload, signature, SECRET)
      url = sso_redirect_url(
          nonce,
          SECRET,
          request.user.email,
          request.user.id,
          request.user.username,
      )
      return redirect('http://discuss.example.com' + url)

Command line
============

To help experiment with the Discourse API, pydiscourse provides a simple
command-line client. It reads the API key from the `DISCOURSE_API_KEY`
environment variable.

.. code:: bash

  export DISCOURSE_API_KEY=your_master_key
  pydiscoursecli --host http://yourhost --api-user system latest_topics
  pydiscoursecli --host http://yourhost --api-user system topics_by johnsmith
  pydiscoursecli --host http://yourhost --api-user system user eviltrout

Run `pydiscoursecli` without arguments to enter an interactive prompt.

Compatibility
=============

This library targets modern Python versions and is continuously tested on a
selection of versions (see the CI badge for details).
