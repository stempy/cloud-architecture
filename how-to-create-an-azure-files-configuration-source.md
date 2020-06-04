# How to create an azure files configuration provider for .NET Core

Asp.NET Cores configuration system is well developed with solid fundamentals and design. 

There is what are called configuration providers which use a configuration source to define how configuration is loaded and read. These can be chained to override main configuration. Rather than go into more detail about configuration now, here is microsofts official documenatation for further reading.

https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-3.1

In this article, I will explain how I wrote a Configuration Provider and Configuration Source to use Azure File(s). Some of the requirements I made for this was:

- no dependency on azure libraries or SDK's to avoid Nuget Versioining issues and the like.
- standard .NET Core conventions and practices. It should just work with the configuration system. This way it could be applied to any .NET Core project that uses standard conventions.
- appsettings.json format, so file(s) were just uploaded at some point, manually via **Azure Storage Explorer** or other means, OR by some process. I actually created a git repository for all configs and set them up in the CI/CD pipeline to deploy.

Now there were several parts to this:

1. Connect to an Azure File(s) resource and obtain a list of config file(s) without the SDK's
2. Download one or more config(s) from azure file(s) and cache locally
3. Parse the JSON files into .NET Core(s) key-value dictionary for .NET Core's configuration provider.
4. Write the Configuration Provider, Configuration Source, and Extension methods to wire everything up.

Now, fortunately for (1) Azure provides REST API's for Azure resources, so it is fairly trivial to work without the SDK's, once this was applied, would get a list of files within a certain resource path in azure files that contained the `appsettings` files.

for (2) this was merely standard http requests to download.

now (3) the JSON parsing, as we were currently using ASP.NET Core 2.1, for the JSON parsing I looked into the ASPNET Core Source for the JSON configuration provider and extracted the JSON parsing out of that. One major difference to bear in mind is .NET Core 2.1 uses the external Newtonsoft.JSON libraries (`Newtonsoft.Json`), with .NET Core 3.1+ they have built fast internal System JSON parsers (`System.Text.Json`) which should be used whenever possible, while newtonsoft is very good, it is much larger and less performant for simple JSON parsing. 

(4) was relatively simple to apply once the others were done.

More to come....
