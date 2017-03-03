# Clients

In cloco a client is analogous to a service account.  This is an account that a machine would use to access cloco and would not be expected to be available to an interactive user.

A client is scoped to a subscription, and each client you create counts towards the user limit in the subscription.

Once you have created a client you will need to generate credentials for that client to be able to access cloco.

## List Clients

```shell
# list subscription clients via the cloco cli
cloco subscription clients list --sub <subscription_id>

# alternatively, use curl:
curl https://api.cloco.io/<subscription_id>/clients --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above command returns JSON array structured like this:

```json
[
  "client-1",
  "client-2"
]
```

This command lets you list all of the clients that have been created in the subscription.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Create or Update Clients

```shell
# to create a client within the subscription using the cloco cli
cloco subscription client create --sub <subscription_id> --name <name>

# alternatively, use curl:
curl -X PUT --data $json https://api.cloco.io/<subscription_id>/clients/<name> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command lets you add a client to the subscription for a cloco user and give them a name.  The client will have access to the subscription as a basic user and must be assigned further clients to be able to access any configuration.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
name | The name of the client. | Required.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Delete Subscription client

```shell
# to delete a client from the subscription using the cloco cli
cloco subscription client delete --sub <subscription_id> --name <name>

# alternatively, use curl:
curl -X DELETE --data $json https://api.cloco.io/<subscription_id>/clients/<name> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

This command lets you delete the client from the subscription via their name.  

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
name | The name of the client. | Required.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## List Client Credentials

```shell
# list the credentials for a client
cloco subscription client credentials list --sub <subscription_id> --name <name>

# via curl:
curl https://api.cloco.io/<subscription_id>/clients/<name>/credentials  --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above curl command returns JSON structured like this:

```json
[
    {
        "generated": "2017-03-03T17:41:58.524Z",
        "key": "9f11a85e648c357c93525a57304cac9646317af8"
    }
]
```

You can list the credentials for a client.  Note that you only get to see the client secret when the credentials are generated.  After this all you can see is the client key.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
name | The name of the client. | Required.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Generate Client Credentials

```shell
# create api credentials vla the cli
cloco subscription client credentials create --sub <subscription_id> --name <name>

# via curl:
curl -X POST --data $json https://api.cloco.io/<subscription_id>/clients/<name>/credentials --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

> The above curl command requires JSON structured like this:

```json
{
  "grant_type": "client_credentials"
}
```

> The above curl command returns JSON structured like this:

```json
{
    "generated": "2017-03-03T17:41:58.524Z",
    "key": "9f11a85e648c357c93525a57304cac9646317af8",
    "secret": "5c382f50c9632eee4b8a86ec98ae800eb7111a4a"
}
```

You can use client credentials with the cloco cli and with the cloco node client.  We recommend you generate different credentials for every environment or service using cloco, and permission them accordingly.  

When credentials are generated we return the client key and the client secret.  The client secret will never be returned to you again, so if you lose it you will need to generate fresh credentials.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
name | The name of the client. | Required.

<aside class="notice">
Requires subscription admin privilege.
</aside>

## Revoke Client Credentials

```shell
# delete api credentials via the cli
cloco subscription client credentials delete --sub <subscription_id> --name <name> --key <cloco_client_key>

# via curl:
curl -X DELETE https://api.cloco.io/<subscription_id>/clients/<name>/credentials/<key> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

If you feel your credentials may have been compromised, or you no longer need them, revoke them as shown.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
sub | The ID of the subscription. | Optional if defaulted via the cloco init command.
name | The name of the client. | Required.
key | The client key from the client credentials. | Required.

<aside class="notice">
Requires subscription admin privilege.
</aside>
