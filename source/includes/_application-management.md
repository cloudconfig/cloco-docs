# Application Management

Within cloco the data stored for an application is metadata - this helps us manage the application data for you and verify that permissions and environments are correct.

## List Applications

```shell
# list applications in a subscription using the cloco cli
cloco application list --sub <subscription_id>

# alternatively, use curl:
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
      "isFavorite": "true"
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

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Save Application

```shell
# to save application metadata using the cloco cli
cloco application put --sub <subscription_id> --app <application_id> --filename <path_to_json_file>

# alternatively, use curl:
curl -X PUT --data @<path_to_json_file> https://api.cloco.io/<subscription_id>/applications/<application_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command requires JSON structured like this:

```json
{
  "applicationIdentifier": "<application_id>",
  "coreDetails": {
    "applicationName": "My-App",
    "description": "This is a description of my app.",
    "isFavorite": "true|false"
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

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.
filename | Path to a JSON document containing the metadata. | Required.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Retrieve an Application

```shell
# retrieve an application using the cloco cli
cloco application get --sub <subscription_id> --app <application_id>

# alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/applications/<application_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON structured like this:

```json
{
  "applicationIdentifier": "<application_id>",
  "coreDetails": {
    "applicationName": "My-App",
    "description": "This is a description of my app.",
    "isFavorite": "true|false"
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

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## Delete an Application

```shell
# delete an application using the cloco cli
cloco application delete --sub <subscription_id> --app <application_id>

# alternatively, use curl:
curl -X DELETE https://api.cloco.io/<subscription_id>/applications/<application_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command deletes the application and any associated configuration stored within cloco.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## List Application Permissions

```shell
# list the application-level permissions via the cloco cli
cloco application permissions list --sub <subscription_id> --app <application_id>

# alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/applications/<application_id>/permissions --header Content-Type:application/json --header "Authorization:Bearer <token>"
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

This command lets you list all of the permissions that are presently assigned at the application level.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## Create or Update Application Permissions

```shell
# to modify application permissions using the cloco cli
cloco application permissions create --sub <subscription_id> --app <application_id> --username <username> [--admin|--read]

# alternatively, use curl:
curl -X POST --data $json https://api.cloco.io/<subscription_id>/applications/<application_id>/permissions --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command requires JSON structured like this:

```json
{
  "identity": "<username>",
  "permissionLevel": "admin|read"
}
```

This command lets you add a cloco user to the application via their username.  

A user granted read rights on the application will be able to read the application information but make no changes to it.

A user granted admin permissions on the application can change application permissions and has write access to the application metadata.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.
username | The username to assign permissions to. | Required.
admin / read | Flag. The permission level to assign. | Optional, defaults to read.

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>

## Revoke Application Permissions

> To remove an application user, use this code:

```shell
cloco application permissions delete --sub <subscription_id> --app <application_id> --username <username>

# alternatively, use curl:
curl -X DELETE --data $json https://api.cloco.io/<subscription_id>/applications/<application_id>/permissions/<username> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command lets you revoke permissions on the application via their user's username.  

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
app | The ID of the application. | Optional if defaulted via the cloco init command.
username | The username to revoke permissions from. | Required.

<aside class="warning">
You cannot remove yourself from the application.  If you want to do this you must first ensure another admin is present within the application and then get them to remove you.
</aside>

<aside class="notice">
Requires subscription admin privilege or application admin privilege.
</aside>
