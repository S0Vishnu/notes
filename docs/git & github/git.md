
## Git **origin** and **upstream**

Once the clone is complete your repo will have a remote named “`origin`” that points to your fork on GitHub.  
Don’t let the name confuse you, this does not point to the original repo you forked from. To help you keep track of that repo we will add another remote named “upstream”:

```bash
$ cd PROJECT_NAME
$ git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
$ git fetch upstream

# then: (like "git pull" which is fetch + merge)
$ git merge upstream/master master

# or, better, replay your local work on top of the fetched branch
# like a "git pull --rebase"
$ git rebase upstream/master
```

There's also a [command-line tool (`gh`) which can facilitate the operations above](https://cli.github.com/manual/gh).

![[Pasted image 20250321123610.png]]

This should be understood in the context of **[GitHub forks](https://help.github.com/articles/fork-a-repo/)** (where you fork a GitHub repo on GitHub before cloning that fork locally).
- `upstream` generally refers to the original repo that you have forked  
    (see also "[Definition of “`downstream`” and “`upstream`”](https://stackoverflow.com/a/2749166/6309)" for more on `upstream` term)
- `origin` is your fork: your own repo on GitHub, clone of the original repo of GitHub

From the GitHub page:

> When a repo is cloned, it has a default remote called `origin` that points to your fork on GitHub, not the original repo it was forked from.  
> To keep track of the original repo, you need to add another remote named `upstream`

```
git remote add upstream https://github.com/<aUser>/<aRepo.git>
```

(with `aUser/aRepo` the reference for the original creator and repository, that you have forked)

Note: [since Sept. 2021](https://github.blog/2021-09-01-improving-git-protocol-security-github/), the unauthenticated git protocol (`git://...`) on port 9418 is no longer supported on GitHub.

You will use `upstream` to **fetch from the original repo** (in order to keep your local copy in sync with the project you want to contribute to).

```
git fetch upstream
```

(`git fetch` alone would fetch from `origin` by default, which is not what is needed here)

You will use `origin` to **pull and push** since you can contribute to your own repository.

```
git pull
git push

git pull --rebase
git rebase --abort 

```

(again, without parameters, 'origin' is used by default)

You will contribute back to the `upstream` repo by making a **[pull request](https://help.github.com/articles/about-pull-requests/)**.


## Git fork

**[Fork](https://help.github.com/articles/fork-a-repo)**, in the GitHub context, doesn't extend Git.  
It only allows clone on the server side.

When you clone a GitHub repository on your local workstation, you cannot contribute back to the upstream repository unless you are explicitly declared as "contributor". That's because your clone is a separate instance of that project. If you want to contribute to the project, you can use forking to do it, in the following way:

- clone that GitHub repository on your GitHub account (that is the ["fork" part](https://help.github.com/articles/fork-a-repo), a clone on the server side)
- contribute commits to that GitHub repository (it is in your own GitHub account, so you have every right to push to it)
- signal any interesting contribution back to the original GitHub repository (that is the **["pull request" part](https://help.github.com/articles/using-pull-requests)** by way of the changes you made on your own GitHub repository)

## GitHub Collaboration Workflow Summary

![[Pasted image 20250321124648.png]]

#### **Getting Started**

1. **Fork**: Create a fork of the main repository on GitHub to work on.
2. **Install Tools**:
    - **Git**: Install and configure Git (e.g., `msysgit` with OpenSSH).
    - Optional: Use GUIs like **TortoiseGit** for convenience.
3. **Clone**: Clone your fork to your local machine:  
    `git clone git@github.com:YOURNAME/Repo.git`
4. **Start Coding**:
    - Use `git stash` to save unfinished work temporarily.
    - Use `git stash apply` to restore stashed changes.

---

#### **Best Practices**

1. **Commit Early and Often**:
    - Break changes into small, logical commits.
    - Use meaningful commit messages.
    - Example:
        
        ```bash
        git add FILE  
        git commit -m "Fix TestCaseXY to pass"
        ```
        
2. **Publish Early and Often**:
    - Push changes to your fork after verifying they compile and pass tests:
        
        ```bash
        git push origin master
        ```


---

#### **Keeping Your Fork Up-To-Date**

1. Add the upstream main repository as a remote:

```bash
git remote add upstream git://github.com/OWNER/Repo.git
```

2. Fetch and merge the latest changes:

```bash
git pull upstream master
```


---

#### **Publishing Contributions**

![[Pasted image 20250321124629.png]]

1. Push your changes to your GitHub fork:

```bash
git push origin master
```

2. Notify the maintainers with a pull request.
3. Ensure your changes:
    - Compile correctly.
    - Pass all tests.

---

#### **Resolving Merge Conflicts**

1. Identify conflicting files:

```bash
git status
```

2. Use tools like **TortoiseGit** or manually edit conflicts.
3. Mark conflicts resolved:

```bash
git add FILE  
git commit -m "Resolve merge conflict"
```


---

#### **Correcting Errors**

1. **Amend Last Commit**:

```bash
git commit --amend
```

2. **Soft Reset**: Undo a commit without discarding changes:

```bash
git reset COMMITHASH
```

3. **Hard Reset**: Revert to a previous state and discard changes:

```bash
git reset --hard COMMITHASH
```

4. **Force Push** (for public remote repos

```bash
  git push -f origin master
```


---

#### **Additional Tools**

- Use `gitk --all` or Git GUI tools to visualize project history.
- Avoid using the **GitHub Fork Queue** to sync changes; use the command-line instead.

By following these steps, you can contribute effectively to GitHub-hosted projects while managing errors and merge conflicts efficiently.


