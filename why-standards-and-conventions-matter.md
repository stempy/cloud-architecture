# Why Standards, Conventions and Automation Matters

One of the most important aspects for scalable software development are standards and conventions.

Things like coding conventions, best practices, design patterns, methodologies and guidelines, industry standards form structure that can be applied and repeated across people, teams and projects in a *consistent* way.

some common responses when it comes to applying standards:

- *"my programs work, and are super clever"*
- *"we have delivery deadlines we need to reach, we dont have time for fluff"*
- *"that wont work with our systems"*
- *"we will get around to that sometime, its in the pipeline"*
- *"we will have to change how we work"*

Not realizing that those patterns are effectively making things harder for themselves in the long term.

Software development has been around for sometime, and while there is many creative avenues it provides, there are many components that form foundational structure of a platform or boilerplate concepts. Hundreds, thousands of very smart people working together to try and improve the the software development processes.

## The solo project

When you first start developing a software application of any kind, and you are the only one working on it, often there can be a lot of experimenting, you want to try different things, some ideas, and concepts. 

You want to be creative and design the system how you see fit. And for small simple applications that a lone developer will work on, this may be fine. 

However here are a few questions to ask:

- how long do you intend to support and maintain this application? 1 month, 6 months, years, decades?
- do other people need to work on this application at some point in the future? 
- do you need to update it to newer versions of the platforms/frameworks as they are released?
- do you want to use tooling to help support development or maintenance of this application?

## What I learned about standards

From my own experience, I have been the sole developer of desktop and web applications that have been used by the public for several years, one particular desktop application that ran 24/7, I maintained for about **5 years** alongside other projects. I have also architectured libraries and applications for other developers to work off

If I had just gone about my way creating things how I saw fit, ignoring common standards and approaches, it would have delayed projects and timelines, as developers would need to be trained or guided with *my* specific implementations. 

I would need to provide documentation and support for however long it existed. It would also be much harder to update to newer versions of the platforms, as there is the migration from version to version, also including all the custom non-standardized approaches to contend with. 

It provides an unnessacary pain point amongst developers as they won't be able to find any community provided documentation on a particular implementation. Developers are also users of what I produce.

As developers we love to experiment, as engineers/architects we have to create systems, processes and methods that can be repeated with consistent results. We will have multiple people working on projects, now if everyone has their own idea about how to go about doing things, than things can become scrambled very quickly. This is why standards are needed, in particular industry standards, developed by people in the field, that know what they are doing.

## Tooling

Another benefit of using standard conventions is tooling and services, patterns designed to support a particular platform or framework.

### Example 1 - GitFlow

Lets take GitFlow as an example, for source control versioning, often considered a successful branching model (since it's release in **2010**) when you apply this standard convention, most *Git* tools can work with it out of the box, no customizations needed, it's implemented in *git* itself.

ie

```sh
git flow init -d
```

Initializes a gitflow based repository using GitFlow as the branching model, with `-d` with all default branch names. Once this is setup, the following tools have features for working with GitFlow without any extra steps, they just work.

- GitKraken
- SourceTree
- Git command line
- others...

### Example 2 - OpenAPI / Swagger

In the legacy world of SOAP based web services, we would use WSDL to get definitions of web services, for XML based data exchange. This provided us with methods to use, their XSL xml schema and other properties, it enabled discovery of services and structures.

In the WebAPI generation, we have OpenAPI/Swagger as the standard for documenting api's, providing model structure (whatever the format)

Effectively providing client consumers of the API, access to human readable documentation in a Consistent structured form.

Now OpenAPI/Swagger standards allow us to:

- *Automatically* generate client model classes and interfaces from a Swagger json output. This alone can virtually eliminate the need to hand-code, hand-update model classes for requests and responses. Which scales in time duration according to the number of models. Have 100 models, these can be re-generated on the fly from swagger. Without swagger it's all updating each one by hand.
- *Import directly* into tools like *postman* for API testing. 

# Automation

Now this goes back to the early days of programming, after all a program is a series of steps to automate something.

A well-crafted software development process is automated whereever possible. Some of the benefits it brings are:

- Less human input, which reduces human error (we are all prone to mistakes)
- less tedious tasks
- consistent, repeatable actions

8/6/2020 more to come....
