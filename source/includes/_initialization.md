# Initializing the cloco cli

```shell
cloco init [--key <cloco_client_key>] [--secret <cloco_client_secret>] [--url <api url>] [--sub <subscription_id>] [--app <application_id>] [--env <environment_id>] [--echo]
```

The cloco cli has a range of options to allow you to set default values that you can use machine-wide.  This is for your convenience, so when you are working with the same subscription, application or environment you don't have to continually enter these details.

All of the parameters are optional, and if not provided they will simply make no change to the configuration.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
key | The client key from the API credentials. | Optional. Must be set once for the cloco cli.  
secret | The client secret from the API credentials. | Optional. Must be set once for the cloco cli.
url | The cloco API url. | Optional.  Will default to the public hosted cloco API.  Set this for on-premise installations.
sub | The ID of the subscription. | Optional.  
app | The ID of the application. | Optional.
env | The ID of the environment. | Optional.
echo | Flag. | Set this to echo the configuration settings to the screen.

<aside class="warning">
You must supply the client key and client secret credentials for the cloco cli to access cloco.
</aside>
