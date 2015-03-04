:title: Shippable API
:description: How to interface with Shppable's API
:keywords: shippable, API, HTTP


.. _api: 

.. note::

  You must be on the Startup or Custom plan to use our API 

API
===========================

** Overview and Purpose **
---------------------------

The Shppable API allows you to do anything through HTTP requests,
that you would normally need to use the Shippable dashboard for. Our
API is RESTful, and can be interfaced through curl, third party tools,
your own wrapper libraries, or any form of HTTPS communication.

Endpoint
^^^^^^^^^^^^^^^^^^^^^^^^^^
The main endpoint for interacting with our API is https://api.shippable.com

Authentication
^^^^^^^^^^^^^^^^^^^^^^^^^^
Using our API requires authenticating with one of your Shippable API
tokens. Shippable API tokens can be generated from your account settings
page.

These tokens must be placed in the header of your HTTP request.
For example, if your API token has the value 10010, you would authenticate
this way with curl:


.. code-block:: bash
  curl -H "Authorization: apiToken 10010" https://api.shippable.com

A useful pattern is to set an env var with the value of your token.
For example, if we saved our token to the environment variable apiToken:

.. code-block:: bash
  curl -H "Authorization: apiToken $apiToken" https://api.shippable.com

This is useful not only because one doesn't have to type tye apiToken in
repeated times, but also allows for secure automization of API scripts; it
is dangerous to directly save your apiToken into code.

.. note::
 NEVER commit code containing your API token to a public repository. Doing
 so will compromise the security of your Shippable account. Treat your
 API token like a password

** Routes **
-----------------------------

/projects
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The /projects route will provide you with information about your projects, 
such as the projects on your Github or Bit Bucket account, and allow you to
interact with them.

**GET /projects**
The /projects route will return JSON presenting various info about your
projects. For example running a GET request such as this through curl

.. code-block:: bash
  curl -H "Authorization: apiToken $apiToken" https://api.shippable.com/projects

Could return a result such as this:


.. code-block:: javascript
  [
    {
      "mostRecentBuild": {
        "createdDate": "2015-02-22T02:52:00.526Z",
        "buildGroupNumber": 1,
        "id": "54e9444fac096311007dccd7",
        "status": 80,
        "durationCumulative": "29930",
        "commitSha": "39d50403945fb5a1d591b5ee2a549806ad80819a",
        "branch": "master"
      },
      "id": "54e80f5d91426fd6a78f6280",
      "language": "ruby",
      "autoBuild": true,
      "fullName": "user/example1",
      "name": "example1",
      "repositoryProvider": "github",
      "branches": [
        "master"
      ],
      "sourceDefaultBranch": "master"
    },
    {
      "mostRecentBuild": null,
      "branches": [],
      "repositoryProvider": "github",
      "name": "example2",
      "fullName": "user/example2",
      "autoBuild": false,
      "id": "54e3c35391426fd6a78cfd41"
    },
  ]

One of the more useful attributes is the id attribute. Knowing this attribute
will allow you configure your project, initiate workflows for your project, and
git more information about it