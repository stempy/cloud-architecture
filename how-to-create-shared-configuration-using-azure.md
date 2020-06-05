# How to create shared configuration using Azure

One of the challenges with creating distributed micro-service based architectures, is configuration, or rather how to apply configuration across applications, for one you need api hosts, database connections, email smtp servers, authentication keys, and various other app related configs. And than you need to cater for different deployment environments, especially in the case of CI/CD which all good software processes follow. Here is one approach I used to overcome this issue.

I will be explaining these processes using `ASP.NET Core 2.1-3.x` as the platform, however many concepts can be applied irrespective of the platform.

When we first start creating an application, for instance a ASP.NET Core WebAPI (or MVC take your pick) we will have some `appsettings` files, one as the base `appsettings.json` and than environment specific ones `appsettings.{env}.json` that will override certain values in the base. So when the application loads it loads the file(s) in order defined by a configuration builder, by default

1. appsettings.json
2. appsettings.{env}.json

now the {env} is gathered from the os users environment variable `ASPNETCORE_ENVIRONMENT`, on a local developers machine typically `development`, than you may have custom ones such as `dev`,`uat`,`stage`,`production` etc to represent the deployed environments.  This is all standard stuff, now the shift is when we have multiple api's or applications that should share the same configurations *without* duplicating config files across every application, keeping it DRY (Dont Repeat Yourself). Otherwise we end up with mismatched configs across different apps and environments, which just becomes a very nasty mess.

Several approaches to sharing configuration across projects:
1. Have a pre|post build or deployment step that syncs configs from a project, or folder to all application directories before deployment.
2. store the configs in a central file store, network path that all applications can read from
3. store configuration in a database of some sort, ie some type of "central" database.

Each have their pros and cons:
1. Pre|post build - when should the scripts sync, if every build, that can be tedious and resouce intensive, or per deployment, which could mean certain config changes wont be picked up until deployment.
2. store configs in central file store, this provides a fairly simple mechanism for all applications to share, they just need access to a network share or folder, ie within a network where the file server is.
3. store config in a database - this sounds nice, however has issues, in that you still need a db connection of some sort, and that db has to contain configs for all environments.

Now, when we had our web applications on a Windows VM running I.I.S, it was very easy to go with option **(2)** and store the configs in a shared network folder path that all the VM's within the datacenter network could use. However once we moved the websites to Azure App Services, this was no longer suitable as app services are their own isolated environment (well within the App Service Plan)....so I decided to go with Azure File(s) to store the configs for ALL applications, and all environments.

see [Azure Files](https://azure.microsoft.com/en-us/services/storage/files/) over at microsoft, it effectively provides a file share based system similar to network file shares, however you can also access it directly with REST based connections, azure tooling such as [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/).

here's the steps taken.

1. Create a git repository for config files, ie `mycompany.configs`
2. place all the `appsettings` files within this repository (note: you can still have explicit ones in each apps directory, configuration with .NET Core is super flexible)
3. create a build/release pipeline in Azure DevOps to upload the configs to an Azure file share that has been created. and apply this on perhaps the `develop` and `master` branches, OR just upload via Azure Storage Explorer.
4. create an azure file(s) configuration data source project, perhaps make it a nuget package uploaded to a private nuget repository. This is what will load the configuration in each app. see [how to create azure files configuration in .NET](how-to-create-an-azure-files-configuration-source.md)
5. for all app services deployed to azure, set the `ASPNETCORE_ENVIRONMENT` variable to the environment name, and check the slot setting checkbox, you really shouldnt need to apply much more than this, as most of the configs will be in the json files uploaded to Azure File(s). Keep it simple.

In our environment, we had a mix of .NET Framework and .NET Core based applications, windows services, desktop apps, command line apps, web apps. With .NET Core configuration, JSON is by default, and easy to apply custom configuration sources such as azure file(s), once I applied this, I than retrofitted a configuration provider for .NET framework, so it could also read the exact same JSON file(s) from the Azure File(s) paths.......it took a few iterations to get just right, however the benefits outweighed the time it took. It dramatically simplified configuration for everything, all apps and services now use shared configuration that is modified in a *single* repository, and automatically deployed upon a push to repo. For the applications to pick up new configs, a simple restart did the trick.

We had an Azure dev account for all dev environments and resources, and a production account for staging and production, this kept things safe and separate. we could create test environments on the fly very simply, and having shared configuration made this very quick and efficient to do.

## An Example appsettings

An Example of an appsettings.json that can cater to multiple applications.

```json
{
    "apiHosts":{
        "authServer":"https://someauth-dev.azurewebsites.net",
        "docConverter":"https://somedocconverter-dev.azurewebsites.net",
        "anotherApi":"https://anotherapi-dev.azurewebsites.net"
    },
    "connectionStrings":{
        "appContext":"",
        "clientDb":"",
        "clientDb-EF6":"",
        "mongoDb":"",
        "couchBase":"",
    },
    "azure":{
        "assetStorageAccount":"",
    },
    "redis":{

    },
    "email":{
        "smtpHost":"",
        "smtpUser":"",
        "smtpPort":"",
        "smtpPassword:""
    }
}

```

Naturally this would depend on your requirements, however most keys would be defined in `appsettings.json` and explicit overrides defined in the `appsettings.{env}.json`.

> **DANGER**: **always** create the lowest level of security in the `appsettings.json`. **NEVER** put PRODUCTION|LIVE settings directly in this file, as it will be used for *ALL* environments, UNLESS explictly overridden, very open to human error. The last thing you want to do is to deploy a dev environment that somehow picks up a PRODUCTION or LIVE connection string for something it shouldnt just because a developer is human and forgot to override it, the responsibility for this comes back to the person who implemented the live settings directly in `appsettings.json` instead of a `appsettings.live.json` or `appsettings.prod.json`...... always infer the safest option by default.

