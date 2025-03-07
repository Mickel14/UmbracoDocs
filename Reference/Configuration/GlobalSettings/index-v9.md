---
versionFrom: 9.0.0
meta.Title: "Umbraco Global Settings"
meta.Description: "Information on the global settings section"
---

# Global Settings

Global settings contains at set of global settings for the CMS such as default UI language, reserved urls, and much more. All of these, except for SMTP settings contains default values, meaning that all configuration is optional, unless you wish to send emails from your site.

The following snippet contains all the available options, with default values, and some example values for the required keys `From`, `Host` and `Port` keys of the SMTP settings:

```json
"Umbraco": {
  "CMS": {
    "Global": {
      "ReservedUrls": "~/.well-known,",
      "ReservedPaths": "~/app_plugins/,~/install/,~/mini-profiler-resources/,~/umbraco/,",
      "TimeOut": "00:20:00",
      "DefaultUILanguage": "en-US",
      "HideTopLevelNodeFromPath": false,
      "UseHttps": false,
      "VersionCheckPeriod": 7,
      "UmbracoPath": "~/umbraco",
      "IconsPath": "~/umbraco/assets/icons",
      "UmbracoCssPath": "~/css",
      "UmbracoScriptsPath": "~/css",
      "UmbracoMediaPath": "~/media",
      "UmbracoMediaPhysicalRootPath": "X:/Shared/Media",
      "InstallMissingDatabase": false,
      "DisableElectionForSingleServer": false,
      "DatabaseFactoryServerVersion": "SqlServer.V2019",
      "MainDomLock": "MainDomSemaphoreLock",
      "MainDomKeyDiscriminator": "",
      "Id": "184a8175-bc0b-43dd-8267-d99871eaec3d",
      "NoNodesViewPath": "~/umbraco/UmbracoWebsite/NoNodes.cshtml",
      "SqlWriteLockTimeOut": "00:00:05",
      "SanitizeTinyMce": false,
      "Smtp": {
        "From": "person@umbraco.dk",
        "Host": "localhost",
        "Port": 25,
        "SecureSocketOptions": "Auto",
        "DeliveryMethod": "Network",
        "PickupDirectoryLocation": "",
        "Username": "person@umbraco.dk",
        "Password": "SuperSecretPassword"
      },
      "DatabaseServerRegistrar": {
        "WaitTimeBetweenCalls": "00:01:00",
        "StaleServerTimeout": "00:02:00"
      },
      "DatabaseServerMessenger": {
        "MaxProcessingInstructionCount": 1000,
        "TimeToRetainInstructions": "2.00:00:00",
        "TimeBetweenSyncOperations": "00:00:05",
        "TimeBetweenPruneOperations": "00:01:00"
      }
    }
  }
}
```

## Root level settings

In the root level section, that is those without a seperate sub section like SMTP, you can configure

### Reserved urls

A comma-seperated list of files to be left alone by Umbraco, these files will be served, and the Umbraco request pipeline will not be triggered.

### Reserved paths

A comma-separated list of all the folders in your directory to be left alone by Umbraco. If you have folders with custom files, add them to this setting to make sure Umbraco leaves them alone.

### Timeout

Configures the number of minutes without any requests being made before the Umbraco user will be required to re-login. Any backoffice request will reset the clock. The format is HH:MM:SS.

### Default UI language

The default language to use in the backoffice if a user isn't explicitly assigned one.

### Hide top level nodes from path

If you are running multiple sites, you don't want the top level node in your URL and can disable it with this setting.

### Use https

Makes sure that all of the requests in the backoffice are called over HTTPS instead of HTTP when set to true.

### Version check period

When this value is set above 0, the backoffice will check for a new version of Umbraco every 'x' number of days where 'x' is the value defined for this setting. Set this value to 0 to never check for a new version.

### Umbraco path

The URL pointing to the Umbraco backoffice, if you change this value you also need to rename the `/umbraco` and `wwwroot/umbraco` folders to match this new name. Be aware that if you run your site on linux the casing needs to be identical.

### Icons path

By adding this value you can specify a new/different folder for storing your icon resources, it's important to be aware of NetCores limitations regarding serving static file content, by default, static content will only be serverd from the wwwroot folder.

### Umbraco CSS path

By adding this you can specify a new/different folder for storing your CSS files, and still be able to edit them within Umbraco. It's also important to be aware of NetCores limitations regarding serving static file content here as well, by default, static content will only be served from the wwwroot folder. For more info see [Extending filesystem](../../../Extending/FileSystemProviders/index.md)

### Umbraco scripts path

By adding this you can specify a new/different folder for storing your script/js files, and still be able to edit them within Umbraco. It's also important to be aware of NetCores limitations regarding serving static file content here as well, by default, static content will only be served from the wwwroot folder. For more info see [Extending filesystem](../../../Extending/FileSystemProviders/index.md)

### Umbraco media path

By adding this you can specify a new/different folder for storing your media files, and still be able to edit them within Umbraco. It's also important to be aware of NetCores limitations regarding serving static file content here as well, by default, static content will only be served from the wwwroot folder. For more info see [Extending filesystem](../../../Extending/FileSystemProviders/index.md)

### Umbraco media physical root path

*Available in V9.3+*

By adding this you can specify a new/different folder for storing your media files elsewhere on the server. Unlike `UmbracoMediaPath`, this does not change the relative path that media is served from (e.g. /media) but allows for files to be stored **outside** of the wwwroot folder. Both relative paths (../../Shared/Media) and absolute server paths (X:/Shared/Media) are supported. For more info see [Extending filesystem](../../../Extending/FileSystemProviders/index.md)

### Install missing database

This is not a setting that commonly needs to be configured.

If enabled Umbraco will try to automatically install the database when it's missing. This is primarily used in conjuction with unattended installs.

### Disable election for single server

This is not a setting that commonly needs to be configured.

This value is primarily used on Umbraco Cloud for a small startup performance optimization. When this is true, the website instance will automatically be configured to not support load balancing and the website instance will be configured to be the 'primary' server for scheduling so no [primary election](https://our.umbraco.com/documentation/Getting-Started/Setup/Server-Setup/load-balancing/file-system-replication.md) occurs. This will save 1 database call during startup.

### Database factory version

This is not a setting that commonly needs to be configured.

This setting is used to specify which sql server version that the database is running, this setting is only required if you use SqlServer 2008, if this is the case set the setting to `"SqlServer.V2008"`

### Main dom lock

Specifies the implementation of `IMainDomLock` to be used.

IMainDomLock is used to synchronize access to various resources e.g. Lucene indexes.

Available options:

+ **MainDomSemaphoreLock** - Windows only, uses a named Semaphore with a `maximumCount` of 1 to control acquisition of MainDom status.
+ **SqlMainDomLock** - Available cross-platform, uses the database to control acquisition of MainDom status.

*Available  in v9.4+*

+ **FileSystemMainDomLock** - Available cross-platform, uses lock files written to LocalTempPath to control acquisition of MainDom status.

The default selection is platform-specific.

On Windows, MainDomSemaphoreLock will be used unless configured otherwise.

On other platforms, SqlMainDomLock will be used unless configured otherwise.

Additionally, SqlMainDomLock will be used when configuration specifies MainDomSemaphoreLock on an unsupported platform.

### Main dom key discriminator

*Available in V9.4+*

For advanced use cases e.g. deployment slot swapping on Azure app services.

When using SqlMainDomLock a MainDomKey is used to identify an instance of a running application.

The MainDomKey is by default comprised of the server's machine name & the application id.

This is generally all that is required to control MainDom status as starting a new process for the same application on the same
server will result in a matching MainDomKey, requiring that an existing instance yields MainDom status to the new process.

Deployment slots for a given Azure app service share the same machine name and without additional configuration will share a
MainDomKey and therefore compete for MainDom status, this can be undesirable if attempting to deploy to a deployment slot
followed by a swap with the production slot as once traffic has switched to the new instance the old production instance reboots
and can re-acquire MainDom status.
See [What happens during a swap](https://docs.microsoft.com/en-us/azure/app-service/deploy-staging-slots#what-happens-during-a-swap).

To prevent this from occurring you can specify a MainDomKeyDiscriminator which should be set as a slot-specific configuration
to prevent the slots from competing for MainDom status.

It's worth noting that during the swap operation there is a period where both instances will share the same
configuration and at this point, the old instance will yield MainDom status to the new instance.

### Main dom release signal polling interval

Gets or sets the duration (in milliseconds) for which the MainDomLock release signal polling task should sleep.
The default value is 2000ms.

### Id

This setting doesn't need to be configured.

This setting contains a unique ID used to identify your project, and is populated the first time your site runs, you shouldn't change this setting.

### No nodes view path

This setting specifies what view to render when there is no content on the site.

### Sql write lock timeout

A timespan value that represents the time to lock the database for a write operation. The setting is not mandatory, but can be used as a fix to extend the timeout if you have been seeing errors in your logs indicating that the default lock timeout is hit. The format for this setting is HH:MM:SS.

## SMTP settings

By adding this settings to the appsettings.json you will be able to send out emails from your Umbraco installation. This could be notifications emails if you are using content workflow, or you are using Umbraco Forms you also need to specify SMTP settings to be able use the email workflows. The forgot password function from the backoffice also needs a SMTP server to send the email with the reset link.

### From

Specifies the default address emails will be sent from, this setting may be overridden some place, such as when inviting a user, where the email of the user sending the invite will be used instead. The format of the address follows the RFC 822 standard so you can include a friendly name using the format `"Friendly Name <your@emailaddress.com>"`

### Host

Address of the SMTP host used to send the email from.

### Port

The port of the SMTP host, port 25 is a common port for SMTP.

### Username

The username used to authenticate with the specified SMTP server, when sending an email.

### Password

The password used to authenticate with the specified SMTP server, when sending an email.

### Secure socket options

Allows you to specify what security should be used for the connection sending the email.

The options are:

* None - No SSL or TLS encryption should be used.
* Auto - Allow the IMailService to decide which SSL or TLS options to use (default). If the server does not support SSL or TLS, then the connection will continue without any encryption.
* SslOnConnect - The connection should use SSL or TLS encryption immediately.
* StartTls - Elevates the connection to use TLS encryption immediately after reading the greeting and capabilities of the server. If the server does not support the STARTTLS extension, then the connection will fail and a NotSupportedException will be thrown.
* StartTlsWhenAvailable - Elevates the connection to use TLS encryption immediately after reading the greeting and capabilities of the server, but only if the server supports the STARTTLS extension.

### Delivery method

Specifies what delivery method should be used for emails, most of the time you'd want to use the default `"Network"` option to send emails over the network. It might be useful during development to use `"SpecifiedPickupDirectory"` to place the email messages in a folder on disk, instead of trying to send them over the network.

### Pickup directory location

If you're using the `"SpecifiedPickupDirectory"` option on as the delivery method, this setting allows you to specify what folder the emails should be saved to.

## Database server registrar settings

It's unlikely that you will have to change these settings unless you're using a load balanced setup.

### Wait time between calls

Sets a value for the amount of time to wait between calls to the database on the background thread.

### Stale server timeout

Sets a value for the time span to wait before considering a server stale, after it has last been accessed.

## Database server messenger

It's unlikely that you will have change these settings, unless you're using a load balanced setup. These settings are all about how load balancing instructions from the database are processed and pruned.

### Max processing instruction

Sets a value for the maximum number of instructions that can be processed at startup; otherwise the server cold-boots (rebuilds its caches).

### Time to retain instructions

Sets a value for the time to keep instructions in the database; records older than this number will be pruned.

### Time between sync operations

Sets a value for the time to wait between each sync operation.

### Time between prune operations

Sets a value for the time to wait between each prune operation.

### Sanitize TinyMce

Gets or sets a value indicating whether TinyMCE scripting sanitization should be applied.

The default value is `false`.
