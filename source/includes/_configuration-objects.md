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
cloco --save-cob --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> --file <path_to_configuration_data> [--mime-type <mime type>] [--passphrase|-p <encryption passphrase>]

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
Note on MIME types:  cloco assumes that you will upload JSON data by default, but you can override this with the --mime-type switch.  For text data we recommend application/x-www-form-urlencoded.
</aside>

<aside class="notice">
Note on encryption:  if the passphrase switch (--passphrase | -p) is used the configuration will be encrypted using AES256 and uploaded as base64-encoded binary.
</aside>

<aside class="notice">
Requires write permission as a configuration object user, else subscription admin privilege or application admin privilege.
</aside>

## Retrieve Configuration Object

> To retrieve configuration data, use this code:

```shell
cloco --get-cob --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> [--passphrase|-p <encryption passphrase>]

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
Note on encryption:  if the passphrase switch (--passphrase | -p) is used the configuration will be decrypted using AES256 and assumes that the configuration data downloaded is base64-encoded binary.
</aside>

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
