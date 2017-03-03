# Introduction

Welcome to the cloco configuration-as-a-service API.  

*cloco* allows developers to build and configure their cloud apps using a configuration API.  The separation of configuration from code is a great first step into building 12-factor applications ([see 12factor.net](http://12factor.net)) and allows you to build a upon rich configuration that's easy to manage via our API.  You can make changes and update your configuration separately from your deployment lifecycle.

This example API documentation page was created with [Slate](https://github.com/lord/slate) and the documentation source is available on [GitHub](https://github.com/cloudconfig/cloco-docs).  If you find any inaccuracies or omissions please join the project and raise a PR.

You can view or follow other cloco projects on GitHub at [https://github.com/cloudconfig](https://github.com/cloudconfig).

## Concepts

### Subscriptions

Subscriptions are the administrative and billing objects within cloco.  Depending on the subscription level, you will have limits on the number of configuration objects you can store, the size of the configuration objects, the version history retained and the number of users.

### Applications

cloco is all about providing a configuration service for applications.  When you create an application in cloco you will be defining a container for the configuration that application will use.

### Environments

cloco is designed to be a single configuration service that you can use to stage your configuration across several environments.  You only need one cloco, as we allow you to store a separate instance of your configuration for each environment.  By default we recommend four environments: dev, test, staging and production.

### Configuration Objects

In cloco, configuration objects represent the data that your application will need in order to configure itself.  We don't need to know - or care - what your configuration data is, we just store it in the database in the most appropriate way.

## Security

### Users

In cloco, users are authenticated by an OAuth provider.  The launch version of cloco supports GitHub authentication but others will follow.  When you authenticate you will be registered in cloco as a user and you will have a personal subscription created for you.

### Clients

cloco is designed for machine access, and so we advise you not to use your administrator credentials for applications that only need to read or write configuration data.  You can create 'clients' in cloco to act as service accounts.  These behave in the same way as users, but they are not authenticated by an OAuth provider.  We recommend that you create clients for each application and assign minimum permissions as appropriate.

A notable difference between clients and users is this:  clients are scoped to a subscription only and cannot be shared across subscriptions.  Users are authenticated people and can access more than one subscription.

### Permissions

Permissions are granted by the subscription administrator(s) to other users in the subscription.  Each user or client can only perform the actions allowed by the administrator.  The most granular permission is to read a single piece of configuration in a single environment.

## Encryption

cloco is a secure configuration store, but don't just take our word for it.  We recommend that cloco configuration documents are encrypted at the client (i.e. at your end), so that we can't even look inside your documents if we wanted to.  We wrap a configuration document inside some metadata so we know where to find it again when you want to retrieve it, but apart from that a configuration document is a transparent blob of data to us.

## Subscription Limits

We want you to use cloco for your apps.  We want to encourage take-up of cloco, but at the same time we need to keep paying for the servers.  You can try a basic version of cloco for free, but if you want to use the enhanced feature of cloco - which we really expect you to have for production use - you'll need to upgrade your version of cloco to a paid version.

Please see the [cloco website](https://www.cloco.io) for more information.
