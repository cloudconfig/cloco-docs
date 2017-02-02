# Subscription Management

In cloco a subscription is where a team of people can manage configuration data together.

##  List Subscriptions

> To list the subscriptions you have access to, use this code:

```shell
cloco --list-subs

# Alternatively, use curl:
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

# Alternatively, use curl:
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

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command retrieves the subscription information.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege.
</aside>

## List Subscription Users

> To list the subscription users, use this code:

```shell
cloco --list-users --sub <subscription_id>

# Alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/users --header Content-Type:application/json --header "Authorization:Bearer <token>"
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

This command lets you list all of the users that are present in the subscription.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Add Subscription User

> To add subscription user, use this code:

```shell
cloco --add-user --sub <subscription_id> -u <username> -r admin|user

# Alternatively, use curl:
curl -X POST --data $json https://api.cloco.io/<subscription_id>/users --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command requires JSON structured like this:

```json
{
  "identity": "<username>",
  "permissionLevel": "admin|user"
}
```

This command lets you add a cloco user to the subscription via their username.  Once the user signs into cloco they will see the subscription in their list of available subscriptions and will be able to use cloco with the required level of privilege.

A user needs to be added to the subscription before they can be granted permissions on either an application or a configuration object.

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Remove Subscription User

> To remove a subscription user, use this code:

```shell
cloco --rm-user --sub <subscription_id> -u <username>

# Alternatively, use curl:
curl -X DELETE --data $json https://api.cloco.io/<subscription_id>/users/<username> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command lets you remove a cloco user from the subscription via their username.  

### URL Parameters

Parameter | Description | Usage
--------- | ----------- | -----
subscription_id | The ID of the subscription. | Optional if defaulted via the --init command
username | The identity of the user to remove.

<aside class="warning">
You cannot remove yourself from the subscription.  If you want to do this you must first ensure another admin is present within the subscription and then get them to remove you.
</aside>

<aside class="notice">
Requires subscription admin privilege.
</aside>
