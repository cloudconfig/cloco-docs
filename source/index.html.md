---
title: cloco API reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://www.cloco.io'>cloco.io</a>
  - <a href='https://www.345.systems'>cloco brought to you by 345</a>
  - <a href='https://github.com/cloudconfig/cloco-docs'>Contribute to this documentation.</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the cloco configuration-as-a-service API.  

*cloco* allows developers to build and configure their cloud apps using a configuration API.  The separation of configuration from code is a great first step into building 12-factor applications ([see 12factor.net](http://12factor.net)) and allows you to build a upon rich configuration that's easy to manage via our API.  You can make changes and update your configuration separately from your deployment lifecycle.

This example API documentation page was created with [Slate](https://github.com/lord/slate) and the documentation source is available on [GitHub](https://cloudconfig/cloco-docs).  If you find any inaccuracies or omissions please join the project and raise a PR.

## Concepts

### Subscriptions

Subscriptions are the administrative and billing objects within cloco.  Depending on the subscription level, you will have limits on the number of configuration objects you can store, the size of the configuration objects, the version history retained and the number of users.

### Applications

cloco is all about providing a configuration service for applications.  When you create an application in cloco you will be defining a container for the configuration that application will use.

### Environments

cloco is designed to be a single configuratiuon service that you can use to stage your configuration across several environments.  You only need one cloco, as we allow you to store a separate instance of your configuration for each environment.  By default we recommend four environments: dev, test, staging and production.

### Configuration Objects

In cloco, configuration objects represent the data that your application will need in order to configure itself.  We don't need to know - or care - what your configuration data is, we just store it in the database in the most appropriate way.

## Permissions

Permissions are granted by the subscription administrator(s) to other users in the subscription.  Each user can only perform the actions allowed by the administrator.  The most granular permission is to read a single piece of configuration in a single environment.

## Encryption

cloco is a secure configuration store, but don't just take our word for it.  We recommend that cloco configuration documents are encrypted at the client (i.e. at your end), so that we can't even look inside your documents if we wanted to.  We wrap a configuration document inside some metadata so we know where to find it again when you want to retrieve it, but apart from that a configuration document is a transparent blob of data to us.

## Subscription Limits

We want you to use cloco for your apps.  We want to encourage take-up of cloco, but at the same time we need to keep paying for the servers.  You can try a basic version of cloco for free, but if you want to use the enhanced feature of cloco - which we really expect you to have for production use - you'll need to upgrade your version of cloco to a paid version.

Please see the [cloco website](https://www.cloco.io) for more information.

# Installation

## cloco-auth

The utility for authorizing your machine to use cloco is available at [https://github.com/cloudconfig/cloco-auth](https://github.com/cloudconfig/cloco-auth).  Follow the instructions in the README to authentcate and get your machine authorized.

## cloco-bash

The bash utility for cloco is available at [https://github.com/cloudconfig/cloco-bash](https://github.com/cloudconfig/cloco-bash).  Follow the instructions in the README to download and install the utility.

## Setting Defaults

> To set default subscription, application, environment, API URL:

```shell
# To set the default subscription
cloco --init --sub <subscription_id>

# To set the default application
cloco --init --app <application_id>

# To set the default environment
cloco --init --env <environment_id>

# To set the default API URL (for on-premise istallations):
cloco --init --url <api url>
```

Using the cloco bash script you can set defaults for subscription, application, environment, and for use with on-premise installs of cloco, the API URL.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional.  
application_id | The ID of the application. | Optional.
environment_id | The ID of the environment. | Optional.
url | The cloco API url. | Optional.  Will default to the public hosted cloco API.

<aside class="notice">
Note that these are saved against the logged on user's profile, so if you are initializing for an application you will need to execute these using the same user as the application will run under.
</aside>

# User Accounts

## Signup

The registration process for cloco is very easy.  Sign into cloco with GitHub and authorize cloco to read your GitHub profile.  We'll use this to create a user account and a subscription in cloco with the same name as your GitHub username.  

You can log in or sign up for cloco from the cloco website [https://www.cloco.io](https://www.cloco.io) or use the cloco-auth utility.

<aside class="warning">
Currently we only support GitHub login.
</aside>

## Authentication

> To refresh your credentials:

```shell
cloco --refresh-login
```

cloco uses Oauth2 bearer tokens to authenticate on the API, and so you first need to retrieve your bearer token using the cloco-auth application.  This is a small and lightweight utility that allows you to authorize your machine using your GitHub credentials.

Once you have authenticated you'll need to ensure your credentials are refreshed at the start of each session.  Some of the cloco clients may do this for you.

## My Details

> To check your account information, use this code:

```shell
cloco --me

# Alternatively, use curl:
curl https://api.cloco.io/me  --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

To check the cloco account you're logged in under, use the /me route.

# Subscription Management

In cloco a subscription is where a team of people can manage configuration data together.

##  List Subscriptions

> To list the subscriptions you have access to, use this code:

```shell
cloco --list-subs

# Alternatively, use curl:
curl https://api.cloco.io  --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON structured like this:

```json
[
  "Subscription-1",
  "Subscription-2"
]
```

As a user in cloco you can belong to more than one subscription.  If you are a subscription administrator you can add users to your subscription and create applications, otherwise you will only have privilege to perform the actions allowed by your administrator.

## Create a subscription

> To create a new subscription, use this code:

```shell
cloco --create-sub --sub <subscription_id>

# Alternatively, use curl:
curl -X POST https://api.cloco.io/subscription --data $json --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command requires JSON structured like this:

```json
{
  "subscriptionId": "<subscription_id>"
}
```

We are keen to allow people to try cloco for free, so we offer a free basic subscription that you can create if you're registered as a user on cloco.  When you're serious about using cloco in a production application you can upgrade to get larger allowances and enhanced management features.

<aside class="warning">
The subscription identifier must be comprised of only letters, numbers and hyphens.
</aside>

<aside class="notice">
When you create a subscription you will by default be the only user and the subscription administrator.
</aside>

## Retrieve a subscription

> To retrieve a subscription, use this code:

```shell
cloco --get-sub --sub <subscription_id>

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command retrieves the subscription information.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege.
</aside>

## List Subscription Users

> To list the subscription users, use this code:

```shell
cloco --list-users --sub <subscription_id>

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/users --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON array structured like this:

```json
[
  {
    "identity": "<username>",
    "permissionLevel": "admin|user"
  }
]
```

This command lets you list all of the users that are present in the subscription.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Add Subscription User

> To add subscription user, use this code:

```shell
cloco --add-user --sub <subscription_id> -u <username> -r admin|user

# Alternatively, use curl:
curl -X POST --data $json https://api.cloco.io/<subscription_id>/users --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command requires JSON structured like this:

```json
{
  "identity": "<username>",
  "permissionLevel": "admin|user"
}
```

This command lets you add a cloco user to the subscription via their username.  Once the user signs into cloco they will see the subscription in their list of available subscriptions and will be able to use cloco with the required level of privilege.

A user needs to be added to the subscription before they can be granted permissions on either an application or a configuration object.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Remove Subscription User

> To remove a subscription user, use this code:

```shell
cloco --rm-user --sub <subscription_id> -u <username>

# Alternatively, use curl:
curl -X DELETE --data $json https://api.cloco.io/<subscription_id>/users/<username> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command lets you remove a cloco user from the subscription via their username.  

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
username | The identity of the user to remove.

<aside class="warning">
You cannot remove yourself from the subscription.  If you want to do this you must first ensure another admin is present within the subscription and then get them to remove you.
</aside>

<aside class="notice">
Requires subscription admin privilege.
</aside>

# Application Management

Within cloco the data stored for an application is metadata - this helps us manage the application data for you and verify that permissions and environments are correct.

## List Applications

> To list the applications within a subscription, use this code:

```shell
cloco --list-apps --sub <subscription_id>

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/applications  --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON structured like this:

```json
[
  {
    "applicationIdentifier": "<application_id>",
    "coreDetails": {
      "applicationName": "My-App",
      "description": "This is a description of my app.",
      "isDefault": "true"
    },
    "environments": [
      {
        "environmentIdentifier": "dev",
        "environmentName": "Development",
        "description": "Development Environment",
        "isDefault:": "false"
      }
    ],
    "configObjects": [
      {
        "objectIdentifier": "My-Config-Object",
  			"objectName": "My Config Object",
  			"description": "This is a an example configuration object.",
  			"format": "JSON"
      }
    ]
  }
]
```

This command retrieves all of the application metadata within the subscription.  Note that no configuration data is retrieved, but lists of the available configuration objects and environments are returned, along with the application identifier and summary information about the application.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Save Application

> To save application metadata within a subscription, use this code:

```shell
cloco --save-app --sub <subscription_id> --app <application_id> --file <path_to_json_file>

# Alternatively, use curl:
curl -X PUT --data @<path_to_json_file> https://api.cloco.io/<subscription_id>/applications/<application_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command requires JSON structured like this:

```json
{
  "applicationIdentifier": "<application_id>",
  "coreDetails": {
    "applicationName": "My-App",
    "description": "This is a description of my app.",
    "isDefault": "true|false"
  },
  "environments": [
    {
      "environmentIdentifier": "dev",
      "environmentName": "Development",
      "description": "Development Environment",
      "isDefault:": "false"
    }
  ],
  "configObjects": [
    {
      "objectIdentifier": "My-Config-Object",
      "objectName": "My Config Object",
      "description": "This is a an example configuration object.",
      "format": "JSON"
    }
  ]
}
```

This command stores the application metadata in the subscription.  This command will store the application if it does not exist, or will overwrite the application metadata if the application already exists.  

<aside class="warning">
Note that subscription limits on number of applications, number of configuration objects and number of environments will be evaluated when the application data is received, and if these exceed the subscription limits an error will be returned.
</aside>

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Retrieve an Application

> To retrieve an application, use this code:

```shell
cloco --get-app --sub <subscription_id> --app <application_id>

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/applications/<application_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON structured like this:

```json
{
  "applicationIdentifier": "<application_id>",
  "coreDetails": {
    "applicationName": "My-App",
    "description": "This is a description of my app.",
    "isDefault": "true|false"
  },
  "environments": [
    {
      "environmentIdentifier": "dev",
      "environmentName": "Development",
      "description": "Development Environment",
      "isDefault:": "false"
    }
  ],
  "configObjects": [
    {
      "objectIdentifier": "My-Config-Object",
      "objectName": "My Config Object",
      "description": "This is a an example configuration object.",
      "format": "JSON"
    }
  ]
}
```

This command retrieves the application information.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## Delete an Application

> To delete an application, use this code:

```shell
cloco --rm-app --sub <subscription_id> --app <application_id>

# Alternatively, use curl:
curl -X DELETE https://api.cloco.io/<subscription_id>/applications/<application_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command deletes the application and any associated configuration stored within cloco.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## List Application Users

> To list the application users, use this code:

```shell
cloco --list-app-users --sub <subscription_id> --app <application_id>

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/applications/<application_id>/users --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON array structured like this:

```json
[
  {
    "identity": "<username>",
    "permissionLevel": "admin|user"
  }
]
```

This command lets you list all of the users that are presently assigned to the application.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## Add Application User

> To add an application user, use this code:

```shell
cloco --add-app-user --sub <subscription_id> --app <application_id> -u <username>

# Alternatively, use curl:
curl -X POST --data $json https://api.cloco.io/<subscription_id>/applications/<application_id>/users --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command requires JSON structured like this:

```json
{
  "identity": "<username>",
  "permissionLevel": "admin|user"
}
```

This command lets you add a cloco user to the application via their username.  A user added as an application user will be able to list applications they have admin rights to view.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## Remove Application User

> To remove an application user, use this code:

```shell
cloco --rm-app-user --sub <subscription_id> --app <application_id> -u <username>

# Alternatively, use curl:
curl -X DELETE --data $json https://api.cloco.io/<subscription_id>/applications/<application_id>/users/<username> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command lets you remove a cloco user from the application via their username.  

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command
username | The identity of the user to remove.

<aside class="warning">
You cannot remove yourself from the application.  If you want to do this you must first ensure another admin is present within the application and then get them to remove you.
</aside>

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

# Configuration Objects

When you work with configuration objects in the cloco API, you are storing or retrieving configuration data.  This is where you will set the configuration that your applications will consume, or your applications will use these operations to retrieve their configuration.

## List Configuration Objects in Subscription

> To list the configuration objects within a subscription, use this code:

```shell
cloco --list-cobs --sub <subscription_id>

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/configuration  --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON structured like this:

```json
[
  {
      "objectIdentifier":"<object_id>",
      "environmentIdentifier":"<environment_id>",
      "applicationIdentifier":"<application_id>",
      "lastUpdated":"2016-10-24T15:19:36.668Z",
      "lastUpdatedBy":"some@user.com",
      "versionNote":"Saved via API.",
      "created":"2016-10-24T15:19:27.695Z",
      "createdBy":"some@user.com",
      "revisionNumber":2
    }
]
```

This command retrieves the headers (but not the configuration data) of all of the configuration objects saved in the subscription.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege.
</aside>

## List Configuration Objects in Application

> To list the configuration objects within an application, use this code:

```shell
cloco --list-app-cobs --sub <subscription_id> --app <application_id>

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/configuration/<application_id>  --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON structured like this:

```json
[
  {
      "objectIdentifier":"<object_id>",
      "environmentIdentifier":"<environment_id>",
      "applicationIdentifier":"<application_id>",
      "lastUpdated":"2016-10-24T15:19:36.668Z",
      "lastUpdatedBy":"some@user.com",
      "versionNote":"Saved via API.",
      "created":"2016-10-24T15:19:27.695Z",
      "createdBy":"some@user.com",
      "revisionNumber":2
    }
]
```

This command retrieves the headers (but not the configuration data) of all of the configuration objects saved in the application.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## Save Configuration Data

> To save configuration data, use this code:

```shell
cloco --get-app --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> --file <path_to_data_file>

# Alternatively, use curl:
curl -X PUT --data @<path_to_data_file> https://api.cloco.io/<subscription_id>/configuration/<application_id>/<object_id>/<environment_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command does not require the data to be structured, but it will try to parse it as JSON.

This command saves the configuration data within cloco.  We recommend that you encrypt your data before sending it to cloco, and so we expect that we receive will be a base-64 encoded string of encrypted binary.  You can send data in the clear if you wish, for exampe if the data is public-access, and we will parse it as JSON.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command
object_id | The ID of the configuration object. | Required.
environment_id | The ID of the environment. | Optional if defaulted via the --init command

<aside class="notice">
Requires write permission as a configuration object user, else subscription admin privilege or application admin privilege.
</aside>

## Retrieve Configuration Object

> To retrieve configuration data, use this code:

```shell
cloco --get-cob --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id>

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/configuration/<application_id>/<object_id>/<environment_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON structured like this:

```json
{
    "objectIdentifier":"<object_id>",
    "environmentIdentifier":"<environment_id>",
    "applicationIdentifier":"<application_id>",
    "lastUpdated":"2016-10-24T15:19:36.668Z",
    "lastUpdatedBy":"some@user.com",
    "versionNote":"Saved via API.",
    "created":"2016-10-24T15:19:27.695Z",
    "createdBy":"some@user.com",
    "revisionNumber":2,
    "configurationData":"SOME-BASE64-ENCODED-STRING"
  }
```

This command retrieves the configuration object, wrapper in the metadata.  Note that if the configuration data is encrypted then it will be returned as a base-64 encoded string within the object.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command
object_id | The ID of the configuration object. | Required.
environment_id | The ID of the environment. | Optional if defaulted via the --init command

<aside class="notice">
Requires read or write permission as a configuration object user, else subscription admin privilege or application admin privilege.
</aside>

## Retrieve Configuration Object History

> To list the versions of your configuration object, use this code:

```shell
cloco --get-history --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id>

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/configuration/versions/<application_id>/<object_id>/<environment_id>  --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON structured like this:

```json
[
  {
      "objectIdentifier":"<object_id>",
      "environmentIdentifier":"<environment_id>",
      "applicationIdentifier":"<application_id>",
      "lastUpdated":"2016-10-24T15:19:36.668Z",
      "lastUpdatedBy":"some@user.com",
      "versionNote":"Saved via API.",
      "created":"2016-10-24T15:19:27.695Z",
      "createdBy":"some@user.com",
      "revisionNumber":2
    }
]
```

This command retrieves the headers (but not the configuration data) of the configuration object.  The data retrieved will be truncated to the maximum stored history versions supported by your subscription level.  

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command
object_id | The ID of the configuration object. | Required.
environment_id | The ID of the environment. | Optional if defaulted via the --init command

<aside class="notice">
Requires read or write permission as a configuration object user, else subscription admin privilege or application admin privilege.
</aside>

## Retrieve Previous Version

> To retrieve a previous version of your configuration data, use this code:

```shell
cloco --get-version --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> ---version <version>

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/configuration/versions/<application_id>/<object_id>/<environment_id>/<version> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON structured like this:

```json
{
    "objectIdentifier":"<object_id>",
    "environmentIdentifier":"<environment_id>",
    "applicationIdentifier":"<application_id>",
    "lastUpdated":"2016-10-24T15:19:36.668Z",
    "lastUpdatedBy":"some@user.com",
    "versionNote":"Saved via API.",
    "created":"2016-10-24T15:19:27.695Z",
    "createdBy":"some@user.com",
    "revisionNumber":2,
    "configurationData":"SOME-BASE64-ENCODED-STRING"
  }
```

This command retrieves a previous version of your data.  The requested version must exist in the version history.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command
object_id | The ID of the configuration object. | Required.
environment_id | The ID of the environment. | Optional if defaulted via the --init command
version | The version number of the configuration object to roll back to.

<aside class="notice">
Requires read or write permission as a configuration object user, else subscription admin privilege or application admin privilege.
</aside>

## Rollback to Previous Version

> To rollback your configuration data to a previous version, use this code:

```shell
cloco --restore-version --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> ---version <version>

# Alternatively, use curl:
curl -X PUT https://api.cloco.io/<subscription_id>/configuration/versions/<application_id>/<object_id>/<environment_id>/<version> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command does not require any data to be posted in the request body.

This command rolls back your data to a previous version.  The previous history will still be maintained, and a new entry will be inserted into the history to represent the rollback.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command
object_id | The ID of the configuration object. | Required.
environment_id | The ID of the environment. | Optional if defaulted via the --init command
version | The version number of the configuration object to roll back to.

<aside class="warning">
The version needs to exist in the version history.  cloco will read the version and create a new version in the history using the data of the previous version.
</aside>

<aside class="notice">
Requires write permission as a configuration object user, else subscription admin privilege or application admin privilege.
</aside>

## List Configuration Object Users

> To list the configuration object users, use this code:

```shell
cloco --list-cob-users --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id>

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/configuration/<application_id>/<object_id>/<environment_id>/users --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON array structured like this:

```json
[
  {
    "identity": "<username>",
    "permissionLevel": "admin|user"
  }
]
```

This command lets you list all of the users that are presently assigned to the configuration object.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command
object_id | The ID of the configuration object. | Required.
environment_id | The ID of the environment. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>


## Add Configuration Object User

> To add a configuration object user, use this code:

```shell
cloco --add-cob-user --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> -u <username> -r "read|write"

# Alternatively, use curl:
curl -X POST --data $json https://api.cloco.io/<subscription_id>/configuration/<application_id>/<object_id>/<environment_id>/users/<username> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command requires JSON structured like this:

```json
{
  "identity": "<username>",
  "permissionLevel": "admin|user"
}
```

This command lets you add a cloco user to the configuration object via their username.  Note that this applies to a single configuration object in a single application, referencing a single application.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command
object_id | The ID of the configuration object. | Required.
environment_id | The ID of the environment. | Optional if defaulted via the --init command
username | The identity of the user to remove.

<aside class="warning">
You cannot remove yourself from the configuration object.  If you want to do this you must first ensure another admin is present within the application and then get them to remove you.
</aside>

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>


## Remove Configuration Object User

> To remove a configuration object user, use this code:

```shell
cloco --rm-cob-user --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> -u <username>

# Alternatively, use curl:
curl -X DELETE --data $json https://api.cloco.io/<subscription_id>/configuration/<application_id>/<object_id>/<environment_id>/users/<username> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command lets you remove a cloco user from the configuration object via their username.  Note that this applies to a single configuration object in a single application, referencing a single application.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
application_id | The ID of the application. | Optional if defaulted via the --init command
object_id | The ID of the configuration object. | Required.
environment_id | The ID of the environment. | Optional if defaulted via the --init command
username | The identity of the user to remove.

<aside class="warning">
You cannot remove yourself from the configuration object.  If you want to do this you must first ensure another admin is present within the application and then get them to remove you.
</aside>

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>
