# Subscription Management

A cloco subscription is where a team of people can manage configuration data together.

##  List Subscriptions

```shell
# list your subscriptions via the cloco-cli
cloco subscription list

# alternatively, use curl:
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

## Create a Subscription

```shell
# create a subscription via the cloco cli
cloco subscription create --sub <subscription_id>

# alternatively, use curl:
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

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Required.  Must be unique.  If it already exists an error will be raised.

## Retrieve a Subscription

```shell
# retrieve a subscription using the cloco cli
cloco subscription get --sub <subscription_id>

# alternatively, use curl:
curl https://api.cloco.io/<subscription_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command retrieves the subscription information.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Delete a Subscription

```shell
# delete a subscription using the cloco cli
cloco subscription delete --sub <subscription_id>

# alternatively, use curl:
curl -X DELETE https://api.cloco.io/<subscription_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command deletes the subscription and all associated data and configuration.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Required.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## List Subscription Permissions

```shell
# list subscription permissions via the cloco cli
cloco subscription permissions list --sub <subscription_id>

# alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/permissions --header Content-Type:application/json --header "Authorization:Bearer <token>"
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

This command lets you list all of the permissions that are assigned at a subscription level.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Create or Update Subscription Permissions

```shell
# to create or update subscription permissions using the cloco cli
cloco subscription permissions create --sub <subscription_id> --username <username> [--admin|--user]

# alternatively, use curl:
curl -X POST --data $json https://api.cloco.io/<subscription_id>/permissions --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command requires JSON structured like this:

```json
{
  "identity": "<username>",
  "permissionLevel": "admin|user"
}
```

This command lets you add a permissions to the subscription for a cloco user via their username.  Once the user signs into cloco they will see the subscription in their list of available subscriptions and will be able to use cloco with the required level of privilege.

A user needs permissions to be granted on the subscription before they can be granted permissions on either an application or a configuration object.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
username | The username to assign permissions to. | Required.
admin / user | Flag. The permission level to assign. | Optional, defaults to user.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Revoke Subscription Permissions

```shell
# to revoke subscription permissions using the cloco cli
cloco subscription permissions delete --sub <subscription_id> --username <username>

# alternatively, use curl:
curl -X DELETE --data $json https://api.cloco.io/<subscription_id>/permissions/<username> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command lets you revoke permissions on the subscription for a cloco user via their username.  

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
username | The username to revoke permissions from. | Required.

<aside class="warning">
You cannot remove yourself from the subscription.  If you want to do this you must first ensure another admin is present within the subscription and then get them to remove you.
</aside>

<aside class="notice">
Requires subscription admin privilege.
</aside>
