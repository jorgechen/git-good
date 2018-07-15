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

###### Initializing a Git Repository
1. Create a README.md
    1. Title and Summary
    2. Installation
    3. Instructions for Local Development
2. Create a `develop` branch
3. Optionally, go to Pull Requests tab, click on Labels, and create labels such as `DO NOT MERGE`, `WIP`

## Examples

### ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Bad ![#f03c15](https://placehold.it/15/f03c15/000000?text=+)

### ![#c5f015](https://placehold.it/15/c5f015/000000?text=+) Good ![#c5f015](https://placehold.it/15/c5f015/000000?text=+)


* No deployment checks
* No 

## Git and Project Management

Most software companies use project planning tools (JIRA) as part of their SDLC.


## Git and CI/CD

and deployment process (GitLab, Travis).
