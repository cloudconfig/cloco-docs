---
title: cloco API reference

language_tabs:
  - shell
  - curl

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

This example API documentation page was created with [Slate](https://github.com/tripit/slate) and the documentation source is available on [GitHub](https://cloudconfig/cloco-docs).  If you find any inaccuracies or omissions please join the project and raise a PR.

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

# User Accounts

## Signup

> To sign up, use this code:

```shell
# Use the --signup operation to sign up a user with an email address
cloco --signup -u <email> -p <password>
```

```curl
curl -X POST https://api.cloco.io/signup  --data $json --header Content-Type:application/json
```

> The above command requires JSON structured like this:

```json
{
  "username": "<username>",
  "email": "<email>",
  "password": "<password>"
}
```

To use cloco you must first create a user account to allow us to authenticate.  You need to supply an email address and a password, and your email will become your user identity within cloco.

If you're using the raw API you can optionally specify a username.

<aside class="warning">
Your email address must be unique.
</aside>

## Authentication

> To authenticate, use this code:

```shell
# Use the --init operation to log in
cloco --init -u <email> -p <password>
```

```curl
curl -X POST -u <email>:<password> https://api.cloco.io/login  --header Content-Type:application/json
```

cloco uses Oauth2 bearer tokens to authenticate on the API, and so you first need to retrieve your bearer token via the /login route.

## My Details

> To check your account information, use this code:

```shell
cloco --me
```

```curl
curl https://api.cloco.io/me  --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

To check the cloco account you're logged in under, use the /me route.

# Subscription Management

In cloco a subscription is where a team of people can manage configuration data together.

##  List Subscriptions

> To list the subscriptions you have access to, use this code:

```shell
cloco --list-subs
```

```curl
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
```

```curl
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
```

```curl
curl https://api.cloco.io/<subscription_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command retrieves the subscription information.

### URL Parameters

Parameter | Description
--------- | -----------
subscription_id | The ID of the subscription.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## List Subscription Users

> To list the subscription users, use this code:

```shell
cloco --list-users --sub <subscription_id>
```

```curl
curl https://api.cloco.io/<subscription_id>/users --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON array structured like this:

```json
[
  {
    "identity": "<email>",
    "permissionLevel": "admin|user"
  }
]
```

This command lets you list all of the users that are present in the subscription.

### URL Parameters

Parameter | Description
--------- | -----------
subscription_id | The ID of the subscription.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Add Subscription User

> To add subscription user, use this code:

```shell
cloco --add-user --sub <subscription_id> -u <email>
```

```curl
curl -X POST --data $json https://api.cloco.io/<subscription_id>/users --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command requires JSON structured like this:

```json
{
  "identity": "<email>",
  "permissionLevel": "admin|user"
}
```

This command lets you add a cloco user to the subscription via their email address.  Once the user signs into cloco they will see the subscription in their list of available subscriptions and will be able to use cloco with the required level of privilege.

### URL Parameters

Parameter | Description
--------- | -----------
subscription_id | The ID of the subscription.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Remove Subscription User

> To remove a subscription user, use this code:

```shell
cloco --rm-user --sub <subscription_id> -u <email>
```

```curl
curl -X DELETE --data $json https://api.cloco.io/<subscription_id>/users/<email> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command lets you remove a cloco user from the subscription via their email address.  

### URL Parameters

Parameter | Description
--------- | -----------
subscription_id | The ID of the subscription.
email | The identity email of the user to remove.

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
```

```curl
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

Parameter | Description
--------- | -----------
subscription_id | The ID of the subscription.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Save Application

> To save application metadata within a subscription, use this code:

```shell
cloco --save-app --sub <subscription_id> --app <application_id> --file <path_to_json_file>
```

```curl
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

Parameter | Description
--------- | -----------
subscription_id | The ID of the subscription.
application_id | The ID of the subscription.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Retrieve an Application

> To retrieve an application, use this code:

```shell
cloco --get-app --sub <subscription_id> --app <application_id>
```

```curl
curl https://api.cloco.io/<subscription_id>/applications/<application_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command retrieves the application information.

### URL Parameters

Parameter | Description
--------- | -----------
subscription_id | The ID of the subscription.
application_id | The ID of the subscription.

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## Delete an Application

> To delete an application, use this code:

```shell
cloco --rm-app --sub <subscription_id> --app <application_id>
```

```curl
curl -X DELETE https://api.cloco.io/<subscription_id>/applications/<application_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command deletes the application and any associated configuration stored within cloco.

### URL Parameters

Parameter | Description
--------- | -----------
subscription_id | The ID of the subscription.
application_id | The ID of the subscription.

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## List Application Users

> To list the application users, use this code:

```shell
cloco --list-app-users --sub <subscription_id> --app <application_id>
```

```curl
curl https://api.cloco.io/<subscription_id>/applications/<application_id>/users --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON array structured like this:

```json
[
  {
    "identity": "<email>",
    "permissionLevel": "admin|user"
  }
]
```

This command lets you list all of the users that are present in the subscription.

### URL Parameters

Parameter | Description
--------- | -----------
subscription_id | The ID of the subscription.
application_id | The ID of the subscription.

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## Add Application User

> To add an application user, use this code:

```shell
cloco --add-app-user --sub <subscription_id> --app <application_id> -u <email>
```

```curl
curl -X POST --data $json https://api.cloco.io/<subscription_id>/applications/<application_id>/users --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command requires JSON structured like this:

```json
{
  "identity": "<email>",
  "permissionLevel": "admin|user"
}
```

This command lets you add a cloco user to the application via their email address.  A user added as an application user will be able to list applications they have admin rights to view.

### URL Parameters

Parameter | Description
--------- | -----------
subscription_id | The ID of the subscription.
application_id | The ID of the subscription.

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## Remove Application User

> To remove an application user, use this code:

```shell
cloco --rm-app-user --sub <subscription_id> --app <application_id> -u <email>
```

```curl
curl -X DELETE --data $json https://api.cloco.io/<subscription_id>/applications/<application_id>/users/<email> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command lets you remove a cloco user from the application via their email address.  

### URL Parameters

Parameter | Description
--------- | -----------
subscription_id | The ID of the subscription.
application_id | The ID of the subscription.
email | The identity email of the user to remove.

<aside class="warning">
You cannot remove yourself from the application.  If you want to do this you must first ensure another admin is present within the application and then get them to remove you.
</aside>

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>



# Configuration Objects

## General Configuration Object Management

### List Configuration Objects in Subscription


### List Configuration Objects in Application


### Store Configuration Object


### Retrieve Configuration Object


## Version History


### Retrieve Configuration Object History


### Rollback to Previous Version


## Working with Configuration Object Users

### List Configuration Object Users


### Add Configuration Object User


### Remove Configuration Object User
