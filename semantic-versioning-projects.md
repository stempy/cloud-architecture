# Semantic Versioning for Projects

Versioning has been a topic of much debate over decades, it's a fairly simple concept, give a product or release a version number or name. for instance.

- releaseverion `v1.0.0-somebuild-23223`
- timestamped `201904211900`

Now there is a concept called *semantic versioning*, head on over to http://semver.org/ to find out more. It's a standardized way of versioning with a specific structure.

Now as many projects are versioned using git, there is an amazing tool called [GitVersion](https://gitversion.net/docs/) that is able to automatically create semantic version numbers based on your git repository status, it works well out of the box with standard git workflows such as GitFlow where you always have a `develop` and `master` branch, or GitHub workflow.

For reference

*GitFlow* was introduced in **2010** so most developers are very aware of it.
- [https://nvie.com/posts/a-successful-git-branching-model/](https://nvie.com/posts/a-successful-git-branching-model/)
- [https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

Now, in the case of GitFlow, GitVersion is able to increment version numbers based on commits to `develop`, `feature`, `release`, `hotfix`, `master` branches in a way that works with the gitflow workflow.

5/6/2020
More to come....

