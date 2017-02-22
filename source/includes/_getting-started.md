# Getting Started

## Signup

The registration process for cloco is very easy.  Sign into cloco with GitHub and authorize cloco to read your GitHub profile.  We'll use this to create a user account and a subscription in cloco with the same name as your GitHub username.  

You can log in or sign up for cloco from the cloco website [https://www.cloco.io](https://www.cloco.io).

<aside class="warning">
Currently we only support GitHub login.
</aside>

## Install the cloco-cli

The command line interface for managing cloco is available on pypi.

```shell
# install the cloco cli using pip
pip install cloco-cli
```

## Setting API Credentials

When you register on the [cloco website](https://www.cloco.io) you will need to generate API credentials in order to use the cloco client and the cloco cli.

Once you have obtained your credentials you should initialize the cloco cli with them.  The cloco cli will then use the credentials to retrieve an access token from cloco.

```shell
# set api credentials for the cli to use
cloco init --key <cloco_client_key> --secret <cloco_client_secret>
```

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
key | The client key from the API credentials. | Required for the cloco cli.  
secret | The client secret from the API credentials. | Required for the cloco cli.

## Setting Defaults

```shell
# To set the default subscription
cloco init --sub <subscription_id>

# To set the default application
cloco init --app <application_id>

# To set the default environment
cloco init --env <environment_id>

# To set the default API URL (for on-premise installations):
cloco init --url <api url>
```

Using the cloco cli you can set defaults for subscription, application, environment, and for use with on-premise installs of cloco, the API URL.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional.  
app | The ID of the application. | Optional.
env | The ID of the environment. | Optional.
url | The cloco API url. | Optional.  Will default to the public hosted cloco API.  Set this for on-premise installations.

<aside class="notice">
Note that these are saved under the logged on user's home directory, so if you are initializing for an application you will need to execute these using the same user as the application will run under.
</aside>
