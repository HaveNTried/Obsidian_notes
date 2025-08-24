*24-08-2025 14:55*
### Status: 
Finished Working Frozen
### Tags: [[Git]],[[Commands]]


# git add

The `git add` command adds a change in the working directory to the staging area. It tells Git that you want to include updates to a particular file in the next commit. However, `git add` doesn't really affect the repository in any significant way—changes are not actually recorded until you run [[git commit]].

In conjunction with these commands, you'll also need [[git status]] to view the state of the working directory and the staging area.
## How it works

The `git add` and [[git commit]] commands compose the fundamental Git workflow. These are the two commands that every Git user needs to understand, regardless of their team’s collaboration model. They are the means to record versions of a project into the repository’s history.

Developing a project revolves around the basic edit/stage/commit pattern. First, you edit your files in the working directory. When you’re ready to save a copy of the current state of the project, you stage changes with `git add`. After you’re happy with the staged snapshot, you commit it to the project history with `git commit`. The [git reset](https://www.atlassian.com/git/tutorials/undoing-changes/git-reset) command is used to undo a commit or staged snapshot.

In addition to `git add` and `git commit`, a third command [[git push]] is essential for a complete collaborative Git workflow. `git push` is utilized to send the committed changes to remote repositories for collaboration. This enables other team members to access a set of saved changes.

![Git add snapshot](https://wac-cdn.atlassian.com/dam/jcr:0f27e004-f2f5-4890-921d-65fa77ba2774/01.svg?cdnVersion=2922)

The `git add` command should not be confused with `svn add`, which adds a file to the repository. Instead, `git add` works on the more abstract level of changes. This means that `git add` needs to be called every time you alter a file, whereas `svn add` only needs to be called once for each file. It may sound redundant, but this workflow makes it much easier to keep a project organized.
## Common options

---

```xml
git add <file>
```

Stage all changes in `<file>` for the next commit.

```xml
git add <directory>
```

Stage all changes in `<directory>` for the next commit.

```css
git add -p
```

Begin an interactive staging session that lets you choose portions of a file to add to the next commit. This will present you with a chunk of changes and prompt you for a command. Use `y` to stage the chunk, `n` to ignore the chunk, `s` to split it into smaller chunks, `e` to manually edit the chunk, and `q` to exit.
## Examples

When you’re starting a new project, `git add` serves the same function as `svn import`. To create an initial commit of the current directory, use the following two commands:

```undefined
git add .
git commit
```

Once you’ve got your project up-and-running, new files can be added by passing the path to `git add`:

```undefined
git add hello.py
git commit
```

The above commands can also be used to record changes to existing files. Again, Git doesn’t differentiate between staging changes in new files vs. changes in files that have already been added to the repository.


# References:

- 
  