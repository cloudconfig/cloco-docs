# Configuration Objects

When you work with configuration objects in the cloco API, you are storing or retrieving configuration data.  This is where you will set the configuration that your applications will consume, or your applications will use these operations to retrieve their configuration.

## List Configuration Stored in Application

```shell
# list the configuration stored against an application using the cloco cli
cloco configuration list --sub <subscription_id> --app <application_id>

# alternatively, use curl:
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

<aside class="notice">
This lists the actual configuration stored in the application across all environments.  If you wish to view the available confiuration objects / environments then retrieve the application.
</aside>

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## Store Configuration Data

```shell
# store configuration data using the cloco cli
cloco configuration put --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> [--filename <path_to_configuration_data>] [--data <raw data>] [--mime-type <mime type>]

# alternatively, use curl:
curl -X PUT --data @<path_to_data_file> https://api.cloco.io/<subscription_id>/configuration/<application_id>/<object_id>/<environment_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command does not require the data to be structured, but it will try to parse it as JSON.

This command saves the configuration data within cloco.  We recommend that you encrypt your data before sending it to cloco, and so we expect to receive a base-64 encoded string of encrypted binary.  You can send data in the clear if you wish, for exampe if the data is public-access.  If you specify the MIME type 'application/json' we will parse the configuration as JSON.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.
cob | The ID of the configuration object. | Required.
env | The ID of the environment. | Optional if defaulted via the cloco init command.
filename | The path to the data to upload. | Required if --data not specified.
data | Raw data to upload. | Required if --filename not specified.
mime-type | The MIME type to assign to the data. | Optional, defaults to application/x-www-form-urlencoded.

<aside class="notice">
Note on MIME types:  The cloco cli uploads text data by default, but you can override this with the --mime-type switch.  For text data we recommend application/x-www-form-urlencoded.  For JSON data we recommend application/json.
</aside>

<aside class="notice">
Requires write permission as a configuration object user, else subscription admin privilege or application admin privilege.
</aside>

## Retrieve Configuration Data

```shell
# retrieving configuration using the cloco cli
cloco configuration get --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> [--raw|--json]

# alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/configuration/<application_id>/<object_id>/<environment_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The --raw option will return the configuration data without the metadata.  The --json option returns the JSON metadata as well, structured like this:

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

When we retrieve configuration from the API the configuration data is wrapped in a JSON packet containing metadata.  You have the option to return the raw configuration or to output the JSON wrapper.  Note that if the configuration data is encrypted then it will be returned as a base-64 encoded string.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.
cob | The ID of the configuration object. | Required.
env | The ID of the environment. | Optional if defaulted via the cloco init command.
raw / json | Flag.  Indicates how to format the response. | Optional, defaults to raw.

<aside class="notice">
Requires read or write permission as a configuration object user, else subscription admin privilege or application admin privilege.
</aside>

## Note on Encryption

```shell
# to upload encrypted data using openssl and the cloco cli
CLOCO_ENCRYPTION_KEY="enter your encryption key here"
FILENAME="enter the path to the file you wish to upload"
CONFIG_OBJECT="your configuration object identifier"
cloco configuration put --cob $CONFIG_OBJECT --data $(openssl enc -aes256 -a -A -nosalt -in $FILENAME -k $CLOCO_ENCRYPTION_KEY)
```

```shell
# to download and decrypt data using openssl and the cloco cli
cloco configuration get --cob $CONFIG_OBJECT --raw  | openssl enc -aes256 -d -a -nosalt -k $CLOCO_ENCRYPTION_KEY
```

We have designed cloco to be a secure configuration store.  However, we advise you to encrypt your configuration data before sending it to cloco, and to decrypt your data after downloading it.  In this section we provide examples on how you can encrypt and decrypt your data, but treat these as an illustration only.  Make sure you understand your encryption and choose the algorithm(s) most suited to your needs.

In these examples the subscription, application and environment have all been set using cloco init:

    `$ cloco init --sub subscription --app application --env environment`

<aside class="warning">
It is your responsibility to encrypt your configuration before you send it to cloco.  Please keep your encryption keys safe - if you lose them we will not be able to recover your data for you.
</aside>

## List Configuration Versions

```shell
# list configuration versions usign the cloco cli
cloco configuration version list --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id>

# alternatively, use curl:
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

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.
cob | The ID of the configuration object. | Required.
env | The ID of the environment. | Optional if defaulted via the cloco init command.

<aside class="notice">
Requires read or write permission as a configuration object user, else subscription admin privilege or application admin privilege.
</aside>

## Retrieve a Configuration Version

```shell
# retrieve a specific version stored in the history using the cloco cli
cloco configuration version get --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> ---version <version>

# alternatively, use curl:
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

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.
cob | The ID of the configuration object. | Required.
env | The ID of the environment. | Optional if defaulted via the cloco init command.
version | The version number to retrieve. | Required.

<aside class="notice">
Requires read or write permission as a configuration object user, else subscription admin privilege or application admin privilege.
</aside>

## Restore a Configuration Version

```shell
# reinstate a previous version of configuration as the current version using the cloco cli
cloco configuration version restore --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> ---version <version>

# alternatively, use curl:
curl -X PUT https://api.cloco.io/<subscription_id>/configuration/versions/<application_id>/<object_id>/<environment_id>/<version> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command does not require any data to be posted in the request body.

This command rolls back your data to a previous version.  The previous history will still be maintained, and a new entry will be inserted into the history to represent the rollback.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.
cob | The ID of the configuration object. | Required.
env | The ID of the environment. | Optional if defaulted via the cloco init command.
version | The version number to restore. | Required.

<aside class="warning">
The version needs to exist in the version history.  cloco will read the version and create a new version in the history using the data of the previous version.
</aside>

<aside class="notice">
Requires write permission as a configuration object user, else subscription admin privilege or application admin privilege.
</aside>

## List Configuration Permissions

```shell
# list permissions on configuration using the cloco cli
cloco configuration permissions list --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id>

# alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/configuration/<application_id>/<object_id>/<environment_id>/permissions --header Content-Type:application/json --header "Authorization:Bearer <token>"
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

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.
cob | The ID of the configuration object. | Required.
env | The ID of the environment. | Optional if defaulted via the cloco init command.

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>


## Create or Update Configuration Permissions

```shell
# create or update permissions on configuration using the cloco cli
cloco configuration permissions create --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> --username <username> [--read|--write]

# alternatively, use curl:
curl -X POST --data $json https://api.cloco.io/<subscription_id>/configuration/<application_id>/<object_id>/<environment_id>/permissions/<username> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command requires JSON structured like this:

```json
{
  "identity": "<username>",
  "permissionLevel": "admin|user"
}
```

This command lets you grant permissions on configuration via the user's username.  Note that this applies to a single configuration object in a single application, referencing a single environment.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.
cob | The ID of the configuration object. | Required.
env | The ID of the environment. | Optional if defaulted via the cloco init command.
username | The identity of the user to permission. | Required.
read / write | Flag.  Indicates the permission level to assign. | Optional, defaults to read.

<aside class="warning">
You cannot grant yourself permissions on configuration.  If you want to do this you must first ensure another admin is present within the application and then get them to grant you permissions.
</aside>

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## Revoke Configuration Permissions

```shell
# revoke permissions on configuration using the cloco cli
cloco configuration permissions delete --sub <subscription_id> --app <application_id> --cob <object_id> --env <environment_id> --username <username>

# alternatively, use curl:
curl -X DELETE --data $json https://api.cloco.io/<subscription_id>/configuration/<application_id>/<object_id>/<environment_id>/permissions/<username> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command lets you revoke permissions from the configuration via the user's username.  Note that this applies to a single configuration object in a single application, referencing a single environment.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.
cob | The ID of the configuration object. | Required.
env | The ID of the environment. | Optional if defaulted via the cloco init command.
username | The identity of the user to remove. | Required.

<aside class="warning">
You cannot remove yourself from the configuration object.  If you want to do this you must first ensure another admin is present within the application and then get them to remove you.
</aside>

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>
