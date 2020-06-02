# How to prepare for continuos integration and delivery

## Introduction
This document aims to clarify a little about CI/CD, what it is, and how to prepare for it.

## Terms used

- C.I - Continuous Integration - the process of building applications upon merges, pushes etc to check a build works, run unit tests etc
- C.D - Continuous delivery - the process of creating a deployable release, and deploying that to an environment, or several environments.
- Projects - Could be a web application, front end app, mobile, desktop, console, or other application.
- Build - typically the output of a compilation, for instance when an application is compiled it creates what's known as a *"build"*
- Deploy - to send the build up to a web server, or app store, or hosting or some sort, depending on the application type.
- Deployment Environment - before deploying to a live server that the public, or intended audience will use, we create different "Environments" to test the application(s) work, do quality control, user acceptance testing and so on.

Before using C.I and C.D within a software company, there are a few things people would do in order to deploy projects:

- Manual build and deployment
- Scripted build and deployment

### Manually build projects (the very first, most basic approach)
Manual build of a project would be via an IDE, or running a command line to build a project, this would create all the bundled output file(s) for the application, often there would be switches to specify what environment it's to be built for.

Than this application would be deployed one of several ways

If it's a web application
- via FTP/SFTP
- some rcopy/xcopy/robocopy style deployment to a folder on a remote server
- perhaps a web publish process via the IDE, or even command line, which can also do the build in a single step

Or other mobile, desktop, console app
- upload to a publishing system, or store, or file server.

Than can often be many steps involved in building, or deploying an application.
- building a database
- building application components
- running tests

 This introduces an element of human error, as a step may be forgotten, a switch not set correctly or various other things.

There are various approaches to how this is done, however the key point is it's a manual process, someone has to manually build or deploy the application, and this was done for many companies for a long time. This means there is someone who is blocked from doing other things, and often deployments are stalled due to waiting on these deployments. it's a very slow, tedious build-release-test-bug discovery-bugfix-redeploy cycle. Depending on how complex the application components are, especially in a distributed server/client architecture of an application, this could mean very infrequent deployments. Stakeholders wait longer for preview releases, bug fixing is held up, and overall slower delivery times.

This is the very first phase of a software development teams deploy process.

### Scripted build and deployments
The next step up from a manual process is to script parts or almost all of the process, the person building/deploying the app than runs the script from a command line which simplifies the build/deployment process. It could be a one liner script that builds, than another that deploys, or both combined.

This can often be dramatically faster than manual build/deployments and can often reduce the chance of human error (as long as the process is well scripted) as all that's generally required is a simple one line call to build an application.

Infact in 2000 Joel Spolsky (think Trello, Stackoverflow and many others) wrote an article on 12 steps to better code [view here](https://www.joelonsoftware.com/2000/08/09/the-joel-test-12-steps-to-better-code/) and two points reflect this.

- Can you make a build in one step?
- Do you make daily builds?

**Thats how simple a build process should be back in 2000.** - I started my web journey around 2000, and that's how it was then.

Following on from that, building and deploying should be just a one liner.

This is much better than manual build/deployments in many ways, however *someeone* still needs to run these scripts, logon to a build server (or run from their own PC), get into a command line and run them.... perhaps specifing deployment environment etc. It's still time that needs to be repeatedly done.


## Introducing Continuous Integration and Delivery
Ok, so now we have manual and scripted deployments, what's the next step. Well that's C.I (Continuous Integration) and C.D (Continuous Delivery), what these are is effectively *automated* building and deployments. Automated, whoa.....slow down, how is that possible?

Some common responses when first mentioning CI/CD
- "we don't have time for that"
- "we don't have resources for that"
- "we will look into it one day.."
- "application is too simple to need that complexity"
- "its overkill for our needs"

Which is generally a complete lack of understanding about what it is, and how it's done. It's often seen as something that eats into time setting up, when really it gives back time, over and over again. Especially more so as application and team size grows. Its really the only way to successfully scale a team and projects.

Now anyone who has successfully (there is a difference) implemented CI/CD into an organization knows the benefits it brings, and never wants to go backwards to the old approaches that are slow/tedious/error prone.

**Continuos Integration** is basically building an applications codebase upon an event trigger, such as code is merged into a specific branch (such as "develop" in the case of gitflow), this ensures the code builds when merged, than unit tests are run to do automated tests. Now even if there is no unit tests in place, this still builds a compiled application, if there are build errors, this can alert developers of the issue immediately via email, or other form such as slack.

**Continuos Delivery** is the process of creating a release, often from the build artifacts (compiled output) of a CI build. The "build" is created *once* (this is different to how builds are often created manually - more on this later), and than a release can be made for a single environment, or multiple environments, it could be deployed to a "DEV" environment frequently as developers are working on the code, than when it's suitable for another stage of testing, progress to other stages.

> Important: One key difference with CI/CD to manual/scripted processes is CI/CD uses what is called a **Build Once, Deploy Anywhere** strategy as opposed to the **Build For Deployment Environment** strategy often used in manual deployment processes.
>
> **Build Once, Deploy Anywhere** means to create the build independant of the environment it's going to be deployed to, this way it can be deployed to *any* environment. On the deployment cycle, any explicit configuration settings, files etc can be applied for a specific environment.
>
> **Build For Deployment Environment** mean to create a build for an explicit deployment environment including configuration. This build will only work for a single environment, and a completely new build has to be created for another environment.

The **build** and **deployment** are separated, which allows very flexible processes. You could deploy to a dev/testing environment, if that's all goods, simply click a button to push that release to a UAT environment, and later push that to STAGE, or PRODUCTION if needed, you can add approval processes, gateways to check certain conditions are met before going to a certain stage. You could make builds created on certain branches to be automatically deployed to certain environments and so on. It can be as simple or as complex as need be, what it brings however is *consistency, automation, flexibility, safety,auditing, and control*.

If a deployment fails for any reason, you can generally rollback to a previous version within seconds (through use of build/release retention). Try that with a manual process.

## Deployment Environments
Often we have multiple deployment environments to test an application through several stages. Some of the more typical ones are.

- development local debugging environment the developer works with on their local machine.
- DEV deployed dev environment representing the latest "develop" branch (ie in gitflow), often updated very frequently by a CI/CD process.
- UAT User Acceptance Testing, where users go through testing of the app.
- STAGE - the environment just before PRODUCTION to ensure everything is working smoothly, and do any bugfixes that crop up as a result of deployment to this environment, it generally is on the same webserver as production.
- PROD (PRODUCTION/LIVE) - this is the final stage that end-users (private or public) will use.


## Preparing for a CI/CD based environment
There are some essential shifts you need to do for a CI/CD based world

Do's

DO build your application to be independent of any environment as much as possible. Yes there will of course be some things that are environment specific, try and ensure these are configuration or startup based, with the configuration something coming from an external file or data source from the application.

DONT build explicitly for a single environment

You can also view [How I setup CI/CD with Azure DevOps](how-i-setup-ci-cd-with-azure-devops.md)


Some references:
- Build once, deploy everywhere https://medium.com/buildit/build-once-deploy-everywhere-part-1-706d7affaf0f
- Build angular once, deploy anywhere https://dev.to/kylerjohnsondev/build-your-angular-app-once-deploy-anywhere-5ggk