# Git Good: GitHub Usage Conventions 

In an engineering team it's important for us to follow a basic set of standards.  In order to maximize our productivity as developers in a team, it's important to use version control (Git) effectively with several good practices. 


## Master List of Conventions 

* Types of merge
    * Merge
        * Hard to revert 50 commits of a PR
    * Squash merge
        * All code related to a feature or bug combined in a single commit
        * Single commits may become huge
    * Rebase merge
        * No extraneous "Merge Pull" commits clogging history
        * More difficult learning curve

## GitHub Conventions

###### Conventions: Initializing a Git Repository
1. Create a README.md
    1. Title and Summary
    2. Installation
    3. Instructions for Local Development
2. Create a `develop` branch
    1. Go to Settings > Branches
        1. Set the default branch to `develop`
        2. Go to Protected Branches > Choose a Branch > `develop`
        3. Use the following screenshot as guidelines for configuring `develop` and permanent branches:
3. Go to Pull Requests > Labels, and create labels such as `DO NOT MERGE`, `WIP`

## Examples

#### ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Bad Example 1 ![#f03c15](https://placehold.it/15/f03c15/000000?text=+)

TODO George - insert screenshot and link

* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) PR Title is unclear
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) PR Description is empty
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Individual commit messages are unclear
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) There are no reviews before PR was merged
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) No deployment checks

#### ![#c5f015](https://placehold.it/15/c5f015/000000?text=+) Good Example ![#c5f015](https://placehold.it/15/c5f015/000000?text=+)

TODO George

## Git and Project Management

Most software companies use project planning tools like JIRA as part of their SDLC.  Having good habits around working between JIRA and Git will make life easier for developers, QA, project managers, and everyone involved!

###### Conventions: Git and JIRA

* Use the a feature or bug's ticket ID as part of your PR titles and commit messages.  
  * In JIRA, the ticket ID is usually a short string such as `AB-123`, where `AB` is the board your team works on.
* In the PR, add the ticket's URL to the PR description.   

## Git and CI/CD

and deployment process (GitLab, Travis).

## Release Versioning

Semantic Versioning (semver) is the standard way of differentiating our releases.  [Read the full documentation](https://semver.org/)

Semver example: `1.6.20`
  * Major version 1
  * Minor version 6
  * Patch version 20
  * For exceptions, an [optional label can be attached](https://semver.org/#spec-item-11), such as `2.0.0-rc.1` or `-beta`
    
###### Conventions: Semver

* When a new project is created, start at `0.1.0`
* Use `0.x.x` versions for rapid development before incrementing to a major release
  * If we are writing v2, then keep semver to `0.1.x` until we are ready to increment to `2.0.0` 
* In Node, the version is kept inside `package.json` in the `"version": "x.x.x"` attribute
