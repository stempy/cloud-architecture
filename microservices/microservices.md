
# Microservice based architectures

@author: https://github.com/stempy  
date created: 2015  
date edited: 2 April 2021  


## Intro

This provides a successful **working** model of a microservices based web architecture currently used in many commercial applications.

There tends to be confusion on what and how microservices behave from an overall picture.

While this aims to be technology agnostic, some approaches suit specific technologies, for instance contanerization suits docker / kubernetes.

## Topics covered

- Simple microservice architecture
- Multiple microservice layers
- Deployment environments
- Source control and how it relates to deployment processes for microservices based approach
- Configuration across multiple microservices with multiple environments, how it can be handled.
- repository folder structure for microservices


## Simple Microservice Arcthitecture

![Alt text](./microservices_architecture-Microservices Architecture.svg)
<img src="./microservices_architecture-Microservices Architecture.svg">

## Multiple Microservice Layers

<img src="./microservices_architecture-Multiple Microservice Gateways.svg">


## Repository folder structures for microservices


### A Single mono repo for multiple microservices forming a component

An example of a single app component with multiple microservices, with an API gateway to consumers, in a single monorepo

```
+-- SampleApp
    +-- src
    |   +-- SampleApp.Microservice1
    |   |   +-- SampleApp.Microservice1.Api
    |   |   +-- SampleApp.Microservice1.Api.Tests
    |   |   +-- Dockerfile
    |   |   +-- SampleApp.Microservice1.sln
    |   |
    |   +-- SampleApp.Microservice2
    |   |   +-- SampleApp.Microservice2.Api
    |   |   +-- SampleApp.Microservice2.Tests
    |   |   +-- Dockerfile
    |   |   +-- SampleApp.Microservice2.sln
    |   |
    |   +-- SampleApp.ApiGateway
    |   |   +-- SampleApp.ApiGateway.Api
    |   |   +-- SampleApp.ApiGateway.Dal.Ef
    |   |   +-- SampleApp.ApiGateway.Tests
    |   |   +-- Dockerfile
    |   |   +-- SampleApp.ApiGateway.sln
    |
    +-- docker-compose.yaml
    +-- readme.md
```

At a high level folder structure with multiple app components

- Multiple Microservices based monorepos
- Web UI



```
+-- repos
|
|   +-- SampleApp
|       +-- src
|       +-- docker-compose.yaml
|
|   +-- SampleApp2
|       +-- src
|       +-- docker-compose.yaml
|
|   +-- SampleApp.WebUI
|       +-- src
|       +-- docker-compose.yaml
```
