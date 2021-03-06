# How to build web applications in 2020

I created this because of the general lack of understanding when it comes to building web applications using standard practices in 2020. There are basic concepts, such as components of a web application, api structures, design patterns, rest api's, configuration, distributed micro-services. There is a lot of information out there often leading to overload, with this I am aiming to provide simple modern approaches to development, using best practices to help build scalable distributed applications. 

## Target Audience

- Developers who wish to improve consistency and standardization across their applications. Implement simple best practices for distributed architectures.
- Team Leads who want to understand best practices for development approaches. Get people on the team working together well.

## Application Basics

- [Why standards,conventions and automation matters](why-standards-and-conventions-matter.md)
- [Basic Components of a Web Application](basic-components-of-a-web-application.md)

## Rest basics

- [Why REST?](why-rest.md)
- [Rest API Basics](rest-api-basics.md)

## Continuous Integration / Continuos Delivery

- [What Azure DevOps provides for CI/CD](what-azure-devops-and-ci-provide.md)
- [Prepare for Continuous Integration And Delivery](how-to-prepare-for-continuous-integration.md)
- [How I setup Azure DevOps for CI/CD](how-i-setup-ci-cd-with-azure-devops.md) for multiple web applications, windows services, command line apps, azure serverless functions across different environments.

## Distributed Microservices Architecture

Considerations such as configuration, deployment environments, authentication, shared libraries, scripts, and tasks.

### Configuration

- [How to create shared configuration using Azure](how-to-create-shared-configuration-using-azure.md). Creating centralized configuration with Azure File(s) or Storage across multiple applications, apis, desktop, command line, server and client applications.
- [how to create azure files configuration in .NET](how-to-create-an-azure-files-configuration-source.md)
- [Injecting configuration based on a users request token](injecting-configuration-per-request.md)

### Authentication

- How to create a simple authentication server for use across applications
- How to componentize API's

### Nuget Shared Libraries

- Building / Packaging / Nuget Libaries
- Deploying Windows Services to Remote Virtual Machines as Nuget Packages

### Scalable Utility Functions

- [Email sending as Azure functions](how-to-handle-email-at-scale.md)
- HTML To PDF conversion using Chrome as a the PDF Renderer for accurate conversion.

## Considerations with Development vs Production Environments

- Sending emails the simple way, testing the process works without delivering real emails in dev
- Database connections
- Configuration

## Releasing Products

- [Semantic Versioning](semantic-versioning-projects.md)
