# Good Practices with Git

In an engineering team it's important for us to follow a basic set of standards.  In order to maximize our productivity as developers in a team, it's important to use version control (Git) effectively with the right conventions. 

These recommendations are **not comprehensive nor mandatory**, but they are a good reference for your team to pick the habits that work for your projects.

### Table of Contents
* [GitHub Recommendations](#github-recommendations)
    * [Initializing a Git Repository](#initializing-a-git-repository)
    * [Creating a Pull Request](#creating-a-pull-request-pr)
    * [Creating and Deleting Branches](#creating-and-deleting-branches)
* [Types of Merges](#types-of-merges)
    * [Merge](#merge)
    * [Squash](#squash)
    * [Rebase](#rebase)
    * [Learning Resources](#learning-resources)
* [Other Habits](#other-habits)
* [Examples](#examples)
* [Miscellaneous](#miscellaneous)
    * [GUI Tools](#gui-tools)
    * [Git and Project Management](#git-and-project-management)
    * [Release Versioning](#release-versioning)
    * [Git and CI/CD](#git-and-cicd)
    * [GitLab](#gitlab)
* [Tips and Tricks](#tips-and-tricks)

# GitHub Recommendations

### Initializing a Git Repository

1. Create a README.md
    1. Title and Summary
    2. Installation
    3. Instructions for Local Development
2. Create a `develop` branch
    1. Go to Settings > Branches
        1. Set the default branch to `develop`
           * <img src="https://github.com/arundo/git-conventions/blob/develop/images/github-settings-branches.png?raw=true" width="300">
        2. Go to Protected Branches > Choose a Branch > `develop`
        3. This screenshot shows recommendations for configuring the main branch:
           * <img src="https://github.com/arundo/git-conventions/blob/develop/images/github-settings-branch-protection.png?raw=true" width="500">
3. (_Recommended:_) Go to Pull Requests > Labels, and create labels such as `DO NOT MERGE`, `WIP`
4. (_Recommended:_) Set up a [template for your PR](https://help.github.com/articles/creating-a-pull-request-template-for-your-repository/)

### Creating a Pull Request (PR)

To maximize collaboration, it's a team should decide on how to structure your PR process:
- Formatting the PR title
- Formatting the PR description (which is the first comment in the PR that describes its purpose)
- Steps to review and merge it

For example, my team agrees on the following rules:

- Format PR titles to be like `AB-123: Implement feature so and so`
- Use a template for all PRs that contain these sections: 
  - **_Overview_**: 1 or 2 sentences describing the task, with an optional screenshot
  - **_How to Test_**: Steps for a reviewer to test the branch
  - **_TODO_**: A list of items to do, if the PR is still a work in progress.
- Review process:
  - One approval is required before PR can be merged
  - Add a label `DO NOT MERGE` if the PR is still being worked on
  - Use squash merge (see below for more info)

After a PR branch is merged back into the main branch, we recommend deleting that branch (see the Bad Example 4 later in this document).  This keeps our repository more readable! Don't worry, you can restore the branch later from its PR page.

### Creating and Deleting Branches

When starting a feature or bug, create a new branch with an appropriate name.  

Recommendations when naming branches:
* Prepend the branch name with the ticket ID. Example: `AB-123-sort-users`
* If the branch is too generic, such as `bug-fix` or `lint-fix`, you can specify the date. Example: `lint-12-25-2018`  

When starting a feature or bug, I strongly recommend first checking out the 

---

# Types of Merges 

<img src="https://github.com/arundo/git-conventions/blob/develop/images/types-of-merge.png?raw=true" width="400">

In a GitHub PR, there are three types of merging: normal merge, squash merge, and rebase merge.  Below we will summarize each type and list their pros and cons.

![Image: two branches](https://wac-cdn.atlassian.com/dam/jcr:01b0b04e-64f3-4659-af21-c4d86bc7cb0b/01.svg?cdnVersion=ji)

Let's take a master branch and a feature branch.  When we create the PR, how the feature branch's commits gets merged depends on the type of merge.


### Merge

![Image: Merge](https://wac-cdn.atlassian.com/dam/jcr:e229fef6-2c2f-4a4f-b270-e1e1baa94055/02.svg?cdnVersion=ji)

Merge is the default method of moving changes from branch A to branch B.  When multiple people are working in a repo, using merge tends to clutter the main branch and decrease readability.

* Pros
    * Good for rapid prototyping and throw-away code.
* Cons
    * Merge transfers **all** commit from the source branch. If there are commits with typos or have unclear messages, those gets spammed into the main branch too
    * Merge also appends **an extra** "Merge" commit in the commit history.  This adds to the clutter when there are many people merging into `develop`.
      * If a feature PR has 50 commits with an extra "Merge" commit merging into `develop` will flood its commit history.  It usually obscures the commit history of the main development branch, making it hard to find a change.  
      * If a [PR has a single commit](https://github.com/arundo/git-conventions/pull/2), using merge will **still** add a ["Merge" commit](https://github.com/arundo/git-conventions/pull/2).
    * It's more difficult to [revert a feature](https://stackoverflow.com/questions/7099833/how-to-revert-a-merge-commit-thats-already-pushed-to-remote-branch), then change it, then try to re-merge the reverted feature branch.
* *Recommendation:* 
    * Try to avoid merging PRs unless you are prototyping.  
    * In long-term projects, you can even disable the first merge option in GitHub Settings:
      <img src="https://github.com/arundo/git-conventions/blob/develop/images/github-settings-disable-merge.png?raw=true" width="400">

### Squash

<img src="https://github.com/arundo/git-conventions/blob/update-readme/images/squash-merge.png?raw=true">

A squash merge combines all the changes you made to a feature branch into a single commit.  This single commit then gets merged into your main branch.

With squash merge, 1 PR === 1 commit in the main branch.  For example, [this PR](https://github.com/arundo/git-conventions/pull/3) with 20+ commits became a [single clear commit](https://github.com/arundo/git-conventions/commits/develop) when we used squash merge.

Squash is minimalist and effective.

* Pros
    * No "Merge" commits cluttering the commit history
    * The main branch's commit history is as compact as possible
    * You can type whatever you want in your feature branch's commits! Commit messages like `lint`, `typo`, `yolo`, `123`, `:` will be squashed into a single commit
        * Note that your PR title should still be descriptive so that the final commit message is clear
        * This solves developers' existential crisis when faced with the herculean effort to make descriptive commit messages every time they change one line of code
* Cons
    * For a big feature, the squashed commit may become huge
* Recommendation:
    * Use squash for most PRs if you want the best of working fast and keeping the repository readable
    * For a big feature, consider rebase instead to separate your PR into a few commits before merging into the main branch

### Rebase

![Image: Rebase](https://wac-cdn.atlassian.com/dam/jcr:5b153a22-38be-40d0-aec8-5f2fffc771e5/03.svg?cdnVersion=ji)

Rebase allows us to "replay our commits" from one branch to another.  Using rebase is a clean way to maintain team projects, because it allows us to fix unclear and redundant commits.  

Once a feature branch is rebased on a development branch, merging the feature into the development branch will simply fast-forward the commit(s) of our new feature.  

NOTE: If you are worth with 2+ people on a feature branch, do not rebase until the end to avoid conflicts!

* Pros
    * No "Merge" commits cluttering the commit history
    * We can clean up commits before pushing into a PR, i.e. edit commit messages, squash minor commits together
    * We can clean up PRs themselves to remove extra "Merge" commits, so that even the feature branch is cleaner
* Cons
    * Greater risk of destroying old commits when pushing rebased changes to an unprotected branch
* **Recommendations:**
    * Use rebase if you want to keep your feature PRs clean
    * Use rebase for bigger features involving thousands of lines of code that may be too big to squash

### Learning Resources

The [Advanced Git Tutorials from Atlassian](https://www.atlassian.com/git/tutorials/advanced-overview) is a great resource for the most common uses, including `merge` vs `rebase`, `reset`, `checkout`, `revert`.  We especially recommend learning about rebase.

Video Tutorials on Rebase:
- [Introduction to Rebase](https://www.youtube.com/watch?v=TymF3DpidJ8)
- [Rebase both CLI and GitKraken](https://www.youtube.com/watch?v=xKanizFigpk)
- [Rebase both CLI and GitKraken (with conflicts)](https://www.youtube.com/watch?v=-3yqteu-pLM)

# Other Habits

### Discuss the PR using comments, not in a private conversation on Slack

Why?
* More people can easily read the comments and help review
* Future maintainers (or you yourself if your memory is like a snail) can understand the discussion

When reviewing a PR, sometimes the discussion occurs in another medium like Slack.  This causes confusion when another developer tries to look at the PR, or anyone tries to look at it later.  You can even forget your own PR's contents after 6 months!

Please discuss in the PR comments.  But if the discuss does happen on Slack, it's easy to take a screenshot and paste directly into the GitHub PR.

### Use labels for a PR

Adding labels to your PR is the easiest way to let reviewers know not to, for example, prematurely merge your PR. For example:
* `DO NOT MERGE` - Do not merge this PR yet
* `POC` - Proof of concept
* `WIP` - Work in progress

<img src="https://github.com/arundo/git-conventions/blob/develop/images/github-pr-labels.png?raw=true" width="400">


# Examples

### ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) [Bad Example 1](https://github.com/arundo/fabric-service-pipelines/pull/97)

![Bad Example 1 Screenshot](https://github.com/arundo/git-conventions/blob/develop/images/bad-pr1-yolo.png?raw=true)

The only positive to this example is that the developer at least recognizes that they are YOLO merging code.
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) PR Title is unclear
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) PR Description is empty
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Individual commit messages are unclear
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) There are no reviews before PR was merged
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) No deployment checks

---

### ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) [Bad Example 2](https://github.com/arundo/fabric-service-pipelines/pull/66)
![Bad Example 2 Screenshot](https://github.com/arundo/git-conventions/blob/develop/images/bad-pr2.png?raw=true)
If you thought it couldn't get worse than Example 1, this developer exceeded your expectations.
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) The PR was merged with no reviews
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) One commit had no letters in its commit

---

### ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) [Bad Example 3](https://github.com/arundo/git-conventions/pull/1)
![Bad Example 3 Screenshot](https://github.com/arundo/git-conventions/blob/develop/images/bad-pr3.png?raw=true)

* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) This PR shows 2 "Merge" commits, which indicates that there were multiple locations where code changed or that you merged from the main `develop` branch twice
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) The first few commits are not related to this PR's title at all, which indicates that the PR branch is not in sync with the main `develop` branch
* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Multiple commits with the same message "Update README"

In this example, it is recommended to squash the PR into a single commit.  You can rebase it too, but in this case rebase is overkill because the PR is small and simple.

---

### ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) [Bad Example 4](https://github.com/arundo/enterprise/branches/all)

<img src="https://github.com/arundo/git-conventions/blob/develop/images/too-many-branches.png?raw=true" width="500">

We should always clean up stale branches after their PRs have been closed.  Keeping active branches allows the team to understand at a glance what features/bugs are being worked on.

---

### ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) [Example 5](https://github.com/arundo/node-api-pipelines/commits/master)

![Bad Example 3 Screenshot](https://github.com/arundo/git-conventions/blob/develop/images/bad5-merge-commits.png?raw=true)

* ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) There are several "Merge" commits cluttering the main branch's commit history.  

---

### ![#00ee11](https://placehold.it/15/00ee11/000000?text=+) [Good Example 1](https://github.com/arundo/enterprise/pulls?utf8=%E2%9C%93&q=)

Good examples of using PRs can be found in the Enterprise UI repository:
<img src="https://github.com/arundo/git-conventions/blob/develop/images/good-list-of-prs-with-ci.png?raw=true" width="500">

* ![#00ee11](https://placehold.it/15/00ee11/000000?text=+) Title contains ticket ID: such as `PS-60`, where PS is the ticket's board and 60 is the ticket's number.
* ![#00ee11](https://placehold.it/15/00ee11/000000?text=+) Title contains a clear description 

### ![#00ee11](https://placehold.it/15/00ee11/000000?text=+) [Good Example 2](https://github.com/arundo/git-conventions/pull/7)

<img src="https://github.com/arundo/git-conventions/blob/develop/images/good-pr2.png?raw=true">

* ![#00ee11](https://placehold.it/15/00ee11/000000?text=+) Clear title
* ![#00ee11](https://placehold.it/15/00ee11/000000?text=+) Clear description
* ![#00ee11](https://placehold.it/15/00ee11/000000?text=+) Comments in the PR
* ![#00ee11](https://placehold.it/15/00ee11/000000?text=+) PR is squashed to avoid commit spam

# Miscellaneous

### GUI Tools

You can do everything on the commandline, but here are a few options for using Git with visualization:

- [GitKraken](https://www.gitkraken.com/)
- [Git Tower](https://www.git-tower.com/features)
- any built-in Git tools in your IDE (VS Code, JetBrains)

### Git and Project Management

Most software companies use project planning tools like JIRA as part of their SDLC.  Having good habits around working between JIRA and Git will make life easier for developers, QA, project managers, and everyone involved!

Guidelines for using GitHub and JIRA:
* Use the a feature or bug's ticket ID as part of your PR titles and commit messages. Example: `AB-123: User Login Page`  
  * In JIRA, the ticket ID is usually a short string such as `AB-123`, where `AB` is the board your team works on.
* In the PR description, link to the ticket's URL.

### Release Versioning

Semantic Versioning (semver) is the standard way of differentiating our releases.  [Read the full documentation](https://semver.org/)

Semver is in the format `$MAJOR_VERSION-$MINOR_VERSION-$PATCH_VERSION`, e.g. `1.6.20`.  Though we do not use this too much, we can also specify an [optional label](https://semver.org/#spec-item-11), such as `2.0.0-rc.1` or `-beta`

Recommendations when using semver:
* When a new project is created, start at `0.1.0`
* Use `0.x.x` versions for rapid development before incrementing to a major release
  * Example: If we are writing v2, then keep semver to `0.1.x` until we are ready to increment to `2.0.0`
* In Node, the version is kept inside [package.json](https://github.com/arundo/fabric-service-stream-ingester/blob/develop/package.json#L3) in the `"version": "x.x.x"` attribute:

```js
{
  "name": "users-service",
  "version": "0.1.0",
  "description": "Example service with a semver"
  ...
}
```

When starting a new feature branch, please update the `"version"` in the branch's first commit, e.g. increment `0.1.45` to `0.2.0`. By doing this, all commits in the feature branch will reflect a new version so that we can tag one to deploy for QA testing if needed.

### Git and CI/CD
Most apps are deployed as services through continuous integration, such as Travis and Circle CI.  

Integrating with CI allows us to automatically run deployment scripts in PRs and on merges.  In our deployment scripts, we check for syntax errors (ESLint) and run unit tests.  We recommend setting your branch to ensure CI checks before allowing a merge:

<img src="https://github.com/arundo/git-conventions/blob/develop/images/github-settings-branch-protection.png?raw=true" width="300">


### GitLab

Most of our conventions apply to GitLab due to its similarity to GitHub.  Note that Pull Requests in GitHub are called Merge Requests in GitLab.

# Tips and Tricks

### Applying patch files

In a few situations, we may end up with a `.diff` or `.patch` file. _WARNING: Avoid if possible! Using diff files are never easy, especially when CRLF settings differ between different machines.  One applicable example is when you're working with a customer that has a firewall between their code base and your working branch - each update from your repo to theirs may require a patch file._

To create a patch on your current branch:

```sh
git format-patch -${NUM_COMMITS} --stdout > some.patch
```

To apply a patch:

```sh
git checkout my-branch
git apply --check --ignore-whitespace some.patch
git am --ignore-whitespace some.patch
```

### Selectively picking commits from another branch

If a PR branch is rejected, but you want to one of its commits in another branch while ignoring all of its other commits, then sometimes you can use `git cherry-pick $COMMIT_HASH`.  _Warning: Do not do this if the original branch will be merged, unless you love merge conflicts!_

### GitHub Saved Replies

GitHub Saved Replies allows you to re-use common markdown.  This is great for power users.  [Read more here.](https://blog.github.com/2016-03-29-saved-replies/)
