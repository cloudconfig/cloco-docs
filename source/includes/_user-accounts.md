# User Accounts

## Signup

The registration process for cloco is very easy.  Sign into cloco with GitHub and authorize cloco to read your GitHub profile.  We'll use this to create a user account and a subscription in cloco with the same name as your GitHub username.  

You can log in or sign up for cloco from the cloco website [https://www.cloco.io](https://www.cloco.io) or use the cloco-auth utility.

<aside class="warning">
Currently we only support GitHub login.
</aside>

## Authentication

> To refresh your credentials:

```shell
cloco --refresh-login
```

cloco uses Oauth2 bearer tokens to authenticate on the API, and so you first need to retrieve your bearer token using the cloco-auth application.  This is a small and lightweight utility that allows you to authorize your machine using your GitHub credentials.

Once you have authenticated you'll need to ensure your credentials are refreshed at the start of each session.  Some of the cloco clients may do this for you.

## My Details

> To check your account information, use this code:

```shell
cloco --me

# Alternatively, use curl:
curl https://api.cloco.io/me  --header Content-Type:application/json --header "Authorization:Bearer <token>"
```

To check the cloco account you're logged in under, use the /me route.
