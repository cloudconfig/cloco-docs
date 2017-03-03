# User Accounts

Each user on cloco has a profile created when they authenticate.  In order to access cloco you must either authenticate directly or use generated credentials.

In this section we explain how you view your profile and manage your credentials.

## My Details

```shell
# retrieve your user profile
cloco me

# alternatively, use curl:
curl https://api.cloco.io/me  --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

To check your cloco profile use the /me route.

## List API Credentials

```shell
# list your api credentials
cloco credentials list

# via curl:
curl https://api.cloco.io/user/credentials  --header Content-Type:application/json --header "Authorization:Bearer <token>"
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

You can list your API credentials that are stored in cloco.  Note that you only get to see the client secret when the credentials are generated.  After this all you can see is the client key.

## Generate API Credentials

```shell
# create api credentials vla the cli
cloco credentials create

# via curl:
curl -X POST --data $json https://api.cloco.io/user/credentials --header Content-Type:application/json --header "Authorization:Bearer <token>"
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

You must use API credentials with the cloco cli.  We recommend you generate different credentials for every environment or service using cloco, and permission them accordingly.  

When credentials are generated we return the client key and the client secret.  The client secret will never be returned to you again, so if you lose it you will need to generate fresh credentials.

## Revoke API Credentials

```shell
# delete api credentials via the cli
cloco credentials delete --key <cloco_client_key>

# via curl:
curl -X DELETE https://api.cloco.io/user/credentials/<cloco_client_key> --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

If you feel your credentials may have been compromised, or you no longer need them, revoke them as shown.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
key | The client key from the API credentials. | Required.

## Authenticate Using API Credentials

```shell
# authenticate via curl:
curl -X POST -u $key:$secret --data $json https://api.cloco.io/oauth/token --header Content-Type:application/json
```

> The above curl command requires JSON structured like this:

```json
{
  "grant_type": "client_credentials"
}
```

The cloco cli and the cloco node client will manage authentication for you once you have initialized the API credentials.  If you are accessing via curl you will need to obtain an access token from the authentication route on the cloco API.

### Parameters

Parameter | Description | Usage
--------- | ----------- | -----
key | The client key from the API credentials. | Required.
secret | The client secret from the API credentials. | Required.
