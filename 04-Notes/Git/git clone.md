*24-08-2025 13:54*
### Status: 
Finished Working Frozen
### Tags: [[Git]], [[Commands]]


# Cloning an existing repository: git clone

---

If a project has already been set up in a central repository, the clone command is the most common way for users to obtain a local development clone. Like `git init`, cloning is generally a one-time operation. Once a developer has obtained a working copy, all [version control](https://bitbucket.org/product/version-control-software) operations are managed through their local repository.

```bash
git clone <repo url>
```

`git clone` is used to create a copy or clone of remote repositories. You pass `git clone` a repository URL. Git supports a few different network protocols and corresponding URL formats. In this example, we'll be using the Git SSH protocol. Git SSH URLs follow a template of: `git@HOSTNAME:USERNAME/REPONAME.git`

An example Git SSH URL would be: `git@bitbucket.org:rhyolight/javascript-data-store.git` where the template values match:

- `HOSTNAME: bitbucket.org`
- `USERNAME: rhyolight`
- `REPONAME: javascript-data-store`

When executed, the latest version of the remote repo files on the main branch will be pulled down and added to a new folder. The new folder will be named after the REPONAME in this case `javascript-data-store`. The folder will contain the full history of the remote repository and a newly created main branch.

---
## Extended git clone

Here we'll examine the `git clone` command in depth. `git clone` is a Git command line utility which is used to target an existing repository and create a clone, or copy of the target repository. In this page we'll discuss extended configuration options and common use cases of `git clone`. Some points we'll cover here are:

- Cloning a local or remote repository
- Cloning a bare repository
- Using shallow options to partially clone repositories
- Git URL syntax and supported protocols

### Repo-to-repo collaboration

It’s important to understand that Git’s idea of a “working copy” is very different from the working copy you get by checking out code from an SVN repository. Unlike SVN, Git makes no distinction between the working copy and the central repository—they're all full-fledged [Git repositories](https://bitbucket.org/product/code-repository).

This makes collaborating with Git fundamentally different than with SVN. Whereas SVN depends on the relationship between the central repository and the working copy, Git’s collaboration model is based on repository-to-repository interaction. Instead of checking a working copy into SVN’s central repository, you [[git push|push]] or [[git pull|pull]] commits from one repository to another.
![Git Tutorial: Repo to Working Copy Collaboration](https://wac-cdn.atlassian.com/dam/jcr:e5228129-76b1-4b2c-8f10-af789f2ea6c0/03.svg?cdnVersion=2922)

![Git Tutorial: Repo to Repo Collaboration](https://wac-cdn.atlassian.com/dam/jcr:5d68ce55-59a7-4840-a896-eb2014a9f17b/02.svg?cdnVersion=2922)

Of course, there’s nothing stopping you from giving certain Git repos special meaning. For example, by simply designating one Git repo as the “central” repository, it’s possible to replicate a [centralized workflow](https://www.atlassian.com/git/tutorials/comparing-workflows) using Git. The point is, this is accomplished through conventions rather than being hardwired into the VCS itself.

## Configuration options


### git clone -branch

The `-branch` argument lets you specify a specific branch to clone instead of the branch the remote `HEAD` is pointing to, usually the main branch. In addition you can pass a tag instead of branch for the same effect.

```bash
git clone --branch
```

### git clone -mirror vs. git clone -bare

#### git clone --bare

Similar to `git init --bare,` when the `-bare` argument is passed to `git clone,` a copy of the remote repository will be made with an omitted working directory. This means that a repository will be set up with the history of the project that can be pushed and pulled from, but cannot be edited directly. In addition, no remote branches for the repo will be configured with the `-bare` repository. Like `git init --bare,` this is used to create a hosted repository that developers will not edit directly.

#### git clone --mirror

Passing the `--mirror` argument implicitly passes the `--bare` argument as well. This means the behavior of `--bare` is inherited by `--mirror`. Resulting in a bare repo with no editable working files. In addition, `--mirror` will clone all the extended refs of the remote repository, and maintain remote branch tracking configuration. You can then run `git remote` update on the mirror and it will overwrite all refs from the origin repo. Giving you exact 'mirrored' functionality.

### Other configuration options

For a comprehensive list of other git clone options visit the [official Git documentation](https://git-scm.com/docs/git-clone). In this document, we'll touch on some other common options.

#### git clone --template

```xml
git clone --template=<template_directory> <repo location>
```
# References:

- 
  