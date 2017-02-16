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
curl -X POST --data $json https://api.cloco.io/<subscription_id>/applications/<application_id>/permissions --header Content-Type:application/json --header "Authorization:Bearer <token>"
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
curl -X DELETE --data $json https://api.cloco.io/<subscription_id>/applications/<application_id>/permissions/<username> --header Content-Type:application/json --header "Authorization:Bearer <token>"
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
