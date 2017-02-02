# NodeJS Client

The cloco node client pulls all the configuration for your application from the cloco API and makes it accessible to your application.  In order to use the cloco node client you will need the following:

- A cloco subscription (account).
- You will have created an application in cloco.
- You will have uploaded configuration for the application.
- To use the shell commands you will have downloaded and installed the [cloco-bash](https://github.com/cloudconfig/cloco-bash) shell.
- You will either have authorized your machine as described above, or you will have generated client credentials.

For a working example of the cloco node client please see the [cloco-node-example](https://github.com/cloudconfig/cloco-node-example) project on GitHub.

## Installation

```shell
npm install cloco-node-client --save
```

Cloco has a NodeJS client available on NPM.  Install this into your node project to integrate cloco seamlessly into your app.

Use the javascript tab on the right to see the examples.

## Options

```javascript
// cloco initialization options.  See below for a description of all the options.
var options = {
  subscription: 'my-cloco-subscription',
  application: 'cloco-cafe',
  environment: 'dev',
  ttl: 60
};
```

The cloco options are where you configure the settings, and therefore how you connect the cloco node client to the cloco API.

Parameter | Description | Usage
--------- | ----------- | -----
application | The ID of the cloco application. | Optional, but if not supplied this will be taken from the machine default (set via [cloco-bash](https://github.com/cloudconfig/cloco-bash)).
cacheCheckInterval | The time in milliseconds between checking for expired configuration in the cache. | Optional, defaults to 5000ms.
credentials | The credentials to use to access cloco. | Optional, but if not supplied will use the machine credentials.  See Credentials section below.
encryptor | The encryption algorithm. | Optional, by default this will be a passthrough encryptor (i.e. will not encrypt).  See Encryption section below for more information.
environment | The deployment environment. | Optional, but if not supplied will use the machine default.
log | The log provider. | Optional.  Defaults to a new bunyan.Logger with basic settings.  See Logging section for more information.
subscription | The name of your cloco subscription.  | Optional, but if not supplied will use the machine default.
ttl | The time in seconds for items to live in the cache before refreshing. | Optional, if not supplied then cache items will never expire.
url | The cloco API url. | Optional.  Will default to the public hosted cloco API.  Use this to override for on-premise installations of cloco.
useDiskCaching | Indicates whether disk caching is used. | Optional.  Will default to false.  When set to true any information received from the API will be saved to disk, and will be loaded from disk on app start if the API cannot be contacted.

## Setting the Subscription

```shell
cloco --init --sub my-cloco-subscription
```

```javascript
options.subscription = 'my-cloco-subscription';
```

You need to specify the identifier of your cloco subscription as the cloco REST API requires this as a URL parameter.  

## Setting the Application

```shell
cloco --init --app my-application
```

```javascript
options.application = 'my-application';
```

You need to specify the identifier of your cloco application as the cloco REST API requires this as a URL parameter.  

## Setting the Environment

```shell
cloco --init --env dev|test|staging|production
```

```javascript
options.environment = 'dev'; // this should be configured per environment.
```

You need to specify the environment that your application is running in, and the appropriate configuration for the environment will be loaded.  You can have different settings depending on the environment.

<aside class="notice">
Your DevOps processes can initialize the environment at deployment time using the shell.
</aside>

## Setting the Credentials

```javascript
// initialize the credentials if supplied via environment variables.
if (process.env.CLOCO_CLIENT_KEY && process.env.CLOCO_CLIENT_SECRET) {
  options.credentials = { key: process.env.CLOCO_CLIENT_KEY, secret: process.env.CLOCO_CLIENT_SECRET };
}
```

You can authorize a machine using the cloco-auth tool to receive a refresh token, in which case credentials are not required to be passed in the options.

If you have not authorized your machine then you need to pass the credentials to the cloco client, and the client will then obtain an access token from the API.

## Setting the API URL

```shell
cloco --init --url https://my-on-premise-cloco-url
```

```javascript
options.url = 'https://my-on-premise-cloco-url';
```

You can run the cloco-node-client against the hosted cloco API or you can obtain your own on-premise or cloud hosted version of cloco.  If you need to change from the default hosted cloco API you can do this by passing in the URL parameter in the options or setting this at a machine level using the shell command.

## Logging

```javascript
// set up the logging.
var bunyan = require('bunyan');
var logger = bunyan.createLogger({name: 'cloco-node-example', level: process.env.CLOCO_LOG_LEVEL || "info"});
options.log = logger;
```

The cloco node client uses bunyan as the default logger.  You can pass in your own instance of bunyan, else the cloco node client will create one for you when it starts up.  Using your own instance is preferable as you can control the verbosity of the logging.

If you wish to use another logging framework other than bunyan you need to make sure that the logger assigned conforms to the bunyan interface, or else you will need to wrap your own logger to make it compliant.

## Encryption

```javascript
// check for the encryption key as an environment variable, and if set then use the encryption option.
if (process.env.CLOCO_ENCRYPTION_KEY) {
  options.encryptor = cloco.createAesEncryptor(process.env.CLOCO_ENCRYPTION_KEY);
}
```

To use cloco without any encryption simply leave the encryptor option undefined.

We recommend that you always encrypt your configuration when you read / write to the API.  In this way we never have access to your configuration data so you can be sure that it is secured.

Cloco ships with an AES256 encryption algorithm as standard, and you can upload new encrypted configuration using the cloco bash shell.  To use the default encryption you just need to supply an encryption key.

### Custom Encryption

If you wish to use your own encryption algorithm, you can write your own code for this and pass it into the options as the encryptor.  Ensure the method dignatures match the required interface.

>To roll your own encryption algorithm, create an encryptor that conforms to the following interface:

````
export interface IEncryptor {

  /**
   * Encrypts the given string into a string.
   * @type {string}
   */
  encrypt(data: string): string;

  /**
   * Decrypts the given string.
   * @param  {string} encrypted The encrypted data.
   * @return {string}           The decrypted data.
   */
  decrypt(encrypted: string): string;

}
````

```javascript
var customEncryptor = {};
customEncryptor.encrypt = function(data) {
    // encrypt in here.
};
customEncryptor.decrypt = function(encrypted) {
    // decrypt in here.
};
options.encryptor = customEncryptor;
```

## Offline Usage

```javascript
// set useDiskCaching to true to use cloco offline.
options.useDiskCaching = true;
```

The cloco client can be used even when access to the cloco API is intermittent or disrupted.  To use this option you should turn on the disk caching option.  Whenever new data is downloaded from the cloco API it will be stored on disk and if the cloco node client is unable to connect to the cloco API the last disk version will be used instead.

```shell
cloco --get-app --sub my-cloco-subscription --app my-application > ~/.cloco/cache/application_my-application

cloco --get-cob --sub my-cloco-subscription --app my-application --cob my-configuration-object --env dev > ~/.cloco/cache/configuration_my-application_my-configuration-object_dev
```

Your DevOps scripts can also prime the client by deploying a version of the configuration onto disk before first use.  In this case you will never need to connect to the API to download the configuration.

Note that the configuration data on disk will use the same encryption as the API.

## Expiry and Refresh

```javascript
options.ttl = 60;  // seconds before an item in cache expires.
options.cacheCheckInterval = 5000;  // milliseconds between checks for expired cache items.
```

cloco anticipates that configuration for an application can change as required during normal running, and this should not require restart of the application.  When an item of configuration is downloaded from the API cloco sets an expiry time on the configuration.  Once the expiry time has expired cloco will attempt to load a fresh copy of the configuration from the API, and if it has changed will updated the version cached in memory.

## Creating the Client

```javascript
var client = cloco.createClient(options);
client.init()
.then(() => {
  logger.debug("cloco client initialized.");
});
```

The node client is created using the options and then initialized to load in the data from the API.  

<aside class="notice">
The init() method on the cloco client is asynchronous and returns a promise.  This is where the data is loaded from the API, and these REST calls are asynchronous.
</aside>

## Subscribing to Client Events

```javascript
client.onConfigurationLoaded.subscribe((sender, arg) => {
  // update an object dynamically
  if (arg.key === "menu") {
    menu = arg.value;
  }

  // change the log level dynamically
  if (arg.key === "logging") {
    logger.level(arg.value.log_level);
  }
});
```

The cloco client has events that you can subscribe to, allowing your application to be notified of changes in the client.

Event | Description
--------- | -----------
onConfigurationLoaded | Fires when new configuration is received from the API.  The configuration is exposed as a key-value pair, the the key being the object identifier of your configuration and the value being the configuration data itself.  The data returned is in the format { key: 'object-identifier', value: { my: 'data' } }.  If the format of your configuration object is marked as JSON the data will be parsed into an object and returned as the value field, else the data will be returned as a string.
onCacheExpired | Fires when an item in cache is marked as stale.  The cloco client will automatically attempt to reload the configuration when this event fires, but you can also subscribe to it.  The argument returned is in the same format as for onConfigurationLoaded.
onError | Fires when an error is encountered during processing.  The cloco client will continue to run, but you can subscribe to this to report any errors.  The argument returned is a JavaScript error object.
