# What Azure DevOps (formerly Visual Studio Team Services) can provide with CI/CD

Azure DevOps provides CI/CD pipelines in the form of *Build* and *Release* using a **Build Once, Deploy Anywhere** strategy, this is a first class principle of CI/CD in that a build for a project is done once, and can than be deployed to *any* environment without rebuilding it, and only applying specific configuration based parameters, or config file(s) as needed.

For instance, lets build a web application.

1. **Build** - the code is compiled, all assets created within the build pipeline, the output is stored as what is known as *artifacts*. The build is created with a build number, and logs all actions.

2. **Release** - a release is created from the artifacts of a *build* pipeline, this release pipeline will contain several "stages" which represent the deployment environments, for example "dev", "testing", "uat", "stage", "production" or "live".... now there could be automated triggers to determine what stages the release is automatically deployed to, in azure devops this is very flexbile. 

We could auto deploy to "dev" upon commits to a "develop" (standard gitflow process) branch or various other automatic deployments. Now once we have tested "dev", we could simply click a button to deploy the exact same release to another stage such as "uat", or "stage", this is all done within seconds, we can also apply what are known as "gates", basically checks before deploying to an environment, such as approval from managers, automated tests to run, again very flexible......and everything is logged and audited.....
Another benefit is *retention history*..... we can keep build and release outputs, incase for instance a deployed release does not work properly when deployed (perhaps a runtime error, db connection is wrong, or anything really) we can very simply redeploy an older release within seconds, or minutes.

## What this means for the business
Well Azure DevOps *properly* configured provides *visibility*.

It shows at a glance, what, when and where something is deployed, and by *who*. You can also find the explicit logs and details of release and builds. NO more fumbling wondering what's where.

What it provided was:
- the release name, and web link url to the azure devops release page
- the environment `[dev]`
- what triggered the release, this could be a build, a manual release etc
- the deployed web url for the application
- the SCM web url for the application, this is the diagnostic panel for the azure app service, not to be confused with the azure portal. for instance it shows processes, webjobs, cpu usage etc for that running app.
- the build agent used to build the release, this could be a Virtual Machine, Desktop PC, or anything the build agent is running on.
- The CD continuos delivery pipeline for the build/release
- Source repository information such as branch and commit id for git

This was consistent across all web applications, there were also other types of applications that provided a similar layout minus the web url's.

I explain more in [How I setup CD/CD With Azure Devops](how-i-setup-ci-cd-with-azure-devops.md). Also there are a few things to consider for CI/CD itself [How to prepare for CI/CD](how-to-prepare-for-continuous-integration.md)

