# Injecting Configuration At a Request Level Scope.

**Different configuration based on actual users token per request**

This is based on ASP.NET Core 2.x-3.x

One of the unique challenges we had with so many environments was being able to inject configuration on a per request level. by this I mean the configuration for database connnections, azure, email etc would be determined based on a users token each request.  Configuration is by default configured once on application startup, and the same configuration injected everywhere in that applications lifetime..

Standard configuration:

- *IConfiguration* is setup in **Program.cs** with the default builders, and any custom config changes can be applied here.
- it is automatically registered as a *singleton* application wide based on the `ASPNETCORE_ENVIRONMENT` variable and injected into *Startup.cs* if you wish to do things with it there.
- such as registering `IOptions<T>` pattern providers.

This is great for application wide configuration, however as we had many environments, and created many environments on the fly (with the CI/CD and Azure setup we could create environments in approx 5-10 minutes using scripts I created), with databases for instance we may only have a few shared instances, "dev", "uat", "stage" and "live", where with actual web applications, we could also have deployments for a specific `feature` branch to test it from a deployed case before merging back to `develop`. Hence we may have a branch `feature/somefeature` where we would create an environment called `somefeature` for fast testing of the specific feature.  Yet we wanted to use the `dev` database for that feature, as opposed to deploying a new database specifically for the feature. OR we may even want to test it with different environments databases and api's.
 
So the solution to this was to have an *environment* variable within the users JWT token claim that would specify the configuration to load per request for that user. ie "env"

So each .NET Application would use `ASPNETCORE_ENVIRONMENT` environment variable as it's static singleton configuration for `IConfiguration` defined in *Program.cs*, and we would use the `IOptions<T>` pattern to inject specific options for a specific environment based on a users token per request.

As an example:

- `ASPNETCORE_ENVIRONMENT` set to `dev`
- deployment slot in azure app service is called `dev`
- url is something suffixed with `[apiappname]-dev.azurewebsites.net`
- release stage name in Azure DevOps is `dev` picked up within a task group within the stage

So this deployed application would look for a `appsettings.json` and `appsettings.dev.json` (plus other provider) to setup `IConfiguration` in program.cs and registered as a singleton application wide. All db connections, azure, email, api hosts etc are for `dev` environment.

Ok, so how do we apply different environments per request?

As mentioned above we would use `IOptions<T>` pattern, however we would go one step further and apply `IOptionsSnapshot`, what this allows us to do is have *named options* which in our case would be the environments themselves. So I created a library which effectively loaded all the configurations for all environment appsettings files within that azure subscription using azure files (as we had separate develop and production subscriptions this was safe) and than setup the `IOptions<T>` to return the specific options for a specific environment based on the users token request, this would mean database, azure, api's etc would automatically be injected in strongly typed models (or EF database context) on a per request level.

This enabled us for instance to have 2 authentication servers. One that created JWT bearer tokens for *all* dev environments, and One for staging and productions. When making a request to the authentication server for a new token, an "env" field was supplied for the environment the request is for. Than the auth server would load the database for that env to authenticate the user against and supply a token with the environment claim in it.... and any resource api using that token would than behind the scenes do it, and there were only one or 2 lines of extra code in each api's *program.cs* to handle this setup.   The rest of the api's just used standard `IOptions<T>` injections... unobtrusive, no mass scale api changes.
