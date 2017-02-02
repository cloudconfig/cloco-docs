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
