# GIT & GitHub for version control
Tivan
2025-05-27

- [<span class="toc-section-number">1</span> Introduction](#sec-intro)
- [<span class="toc-section-number">2</span> Installation &
  acquaintance](#sec-install)
  - [<span class="toc-section-number">2.1</span> Git](#git)
  - [<span class="toc-section-number">2.2</span> GitHub](#github)
- [<span class="toc-section-number">3</span> Initialising & adding
  remote](#sec-init)
- [<span class="toc-section-number">4</span> Be pushy](#sec-push)
- [<span class="toc-section-number">5</span> Don’t
  overcommit](#sec-ignore)

# Introduction

Git is software for version control. Git takes snapshots of the state of
your repository at a specific point in time – these are called
**commits**. Unlike OneDrive, Git does not track revision changes as a
series of modifications (deltas). Git stores commits as snapshots of
entire files that have changed, not as deltas

Git is enhanced for local development because you work on a copy of the
repository on your local machine. This is basically a clone of the
remote repository on a Git server (we’ll be using GitHub, but there are
many other Git server options). Your local Git has the resources and the
snapshots of the revision changes made on these resources all in one
location. These collections of linked snapshots are called **repository
commit history** (repo history for short). This will allow us to work
even without an internet connection on our local and add the changes to
the Git server later on. Git also streamlines nonlinear development
through **branches**, but we won’t delve too deeply into that in this
document.

GitHub is where sharing happens. Although it has many features, the main
ones we’ll be discussing in this document is providing a central
repository that different remote users can draw from and providing
security for the repo histories that we store on it.

For help and information you can visit the [git
documentation](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/).
Or you can run `git --help`.

This document is based mostly on [Happy Git and GitHub for the
useR](https://happygitwithr.com/) written by [Jenny
Bryan](https://jennybryan.org/about/). There is also a great book by
Ponuthorai and Loeliger called *Version Control with Git*.

# Installation & acquaintance

This section walks through setting up Git and GitHub. It also shows how
to get them to talk to each other.

## Git

To check if git is already available on your system, run the command
below.

``` {bash}
git --version
```

If it is not installed, you can use the command below to install it on
Ubuntu. Windows is a bit more complicated. Jenny Bryan has a
[chapter](https://happygitwithr.com/install-git) that walks through the
installation process.

``` {bash}
sudo apt install git-all
```

Once git is installed, you have to introduce yourself. You can use the
sample code below substituting your details for the example details
below. If you are working in Windows, rather work with Git Bash than
PowerShell or

``` {bash}
git config user.name "Peter Kropotkin"
git config user.email "peter@ancom.com"
git config --global --list
```

Set the text editor that will be used if Git kicks you into if you fail
to give it information like forgetting to add a commit message.

``` {bash}
git config --global core.editor "vim"
```

We also want to set our default branch name when we initiate a new
project. It is increasingly becoming popular to name the initial branch
main instead of master.

> In 2020, the init.defaultBranch setting was introduced so that this
> became user-configurable. Shortly thereafter, major Git hosts like
> GitHub and GitLab made main the default initial branch name for repos
> created on their platforms and also provided considerable support for
> renaming existing default branches.

``` {bash}
git config --global init.defaultBranch main
```

## GitHub

GitHub is a remote Git server. Git controls versions, GitHub provides a
central place where versions can be stored and shared. When sending a
request from our local to GitHub, we have to provide credentials to
authenticate ourselves. There are two protocols that we’re able to
communicate with GitHub through: SSH and HTTPS. This document opts for
HTTPS setup because it is easier and has all of the features we require.
If you’re interested in setting up SSH, you can use Jenny’s
[chapter](https://happygitwithr.com/ssh-keys#ssh-keys).

Tokens have replaced password log in – tokens are more secure. We can
generate GitHub tokens [here](https://github.com/settings/tokens). Fine
grained tokens allow more control and I would recommend opting for
those. Make sure to select permissions! The most critical one is
**Contents**.

Once you have generated your token, be sure to copy it and save it
somewhere you won’t loose it. Once you’ve left the page, you won’t be
able to access it again and will have to generate a new token. R has a
useful way to update your GitHub credentials shown in the code chunk
below.

``` {bash}
gitcreds::gitcreds_set()
```

On Linux, to make sure that your PAT persists, you can use the command
below.

``` {bash}
git config --global credential.helper 'cache --timeout=10000000'
```

If you ever feel you need to overwrite a bad credential with a new one,
the easiest way to do this is to call `gitcreds::gitcreds_set()` from R.

Now that we have everything in place, we can connect to GitHub! The
easiest way to demonstrate this and to make sure that the setup went
smoothly is to create a new repository on GitHub and to clone it to a
temporary repository that we can delete afterwards.

- On GitHub, click on Create New Repository
- Copy the HTTPS path of the repository
- In the terminal, navigate to `~/tmp`
- Execute
  `git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git`
  replacing the link with the actual one you copied
- If the repository is in the `~/tmp` directory, it means you’re good to
  go

# Initialising & adding remote

To allow Git to track our project, we have to create a Git repository.
We can do this by navigating to the file and then running `git init`.
The example below shows how to create a new repository and then convert
it to a git repository but `git init` can also be used to convert
existing directories populated with files into Git repositories.

``` {bash}
mkdir ~/projects/my_project
cd ~/projects/my_project
git init -b main
```

Now we have a git repository called `my_project`. We want to add a
.gitignore file to avoid tracking unnecessary files (more about this in
the <a href="#sec-ignore" class="quarto-xref">Section 5</a>). The next
step is to create an empty GitHub repository that we can add as the
remote repository for our local Git repository.

- **Step 1**: go to GitHub, navigate to your repositories and click on
  **New**
- **Step 2**: name the repository on GitHub – ideally we would use the
  same name given to the local Git repository (from the example above it
  would be my_project)
  - Note: do not add a gitignore or README from GitHub. This might
    interfere with our existing repository
- **Step 3**: Click on **Create Repository**
  - GitHub should provide you with a couple of steps for how you go
    about adding the remote repository
- **Step 4**: On our local machine, in the Git repository (my_project),
  run `git commit -m "first commit"` (more on this in
  <a href="#sec-push" class="quarto-xref">Section 4</a>). This will send
  our first snapshot to our local Git
- **Step 5**: copy the link of the remote repository on GitHub and add
  the remote repository by running
  - `git remote add origin https://github.com/your_username/my_project.git`
- **Step 6**: Our local Git should now be linked to the GitHub
  repository. We can push the changes by running the code below
  - `git push -u origin main`
- **Step 7**: refresh the page for the repository on GitHub, you should
  see the files from your local Git repository

# Be pushy

Git is definitive and requires you to be explicit. Unlike software like
OneDrive it does not automatically store the changes you have made. You
have to tell it when to `commit` a change to your local Git, and then
again tell it when to `push` that change to GitHub. I find it best to go
about this using five commands: `git status`, `git pull`, `git add`,
`git commit`, and `git push` (usually in that order).

> If multiple people are working on a project, it is better to
> `git pull` before you commit or push to avoid merge conflicts.

- Start by checking if the local repository is different from the local
  Git and GitHub. We can do this by running `git status`
  - If the status says that your branch is up to date with Git and
    GitHub, you have no issues.
  - If you have changes not staged for commit, it means that you need to
    commit new changes to your local Git
  - 
- We can commit changes to our local Git by using `git commit` and
  `git add`
  - `git add` tells git that there are new files that you want it to
    track (`git add .` will add all files to be tracked)
  - `git commit` is where you send Git a new snapshot. You have to add a
    message with the commit
    `git commit -m "an informative message describing what's changed"`
  - After every git command, I like running `git status` again to see
    how things are progressing
- Now that your changes have been commited, you can use `git push` to
  send them to GitHub

# Don’t overcommit

Some files are like skunks – they are not worth tracking. Such files are
usually large and static, sensitive, or some combination of both. And,
like skunks, they leave you with a rotten smell if you track them.
Either because they expose sensitive information to the world or because
they devour your storage.

> Git offers a way for us to not track these files via the `.gitignore`
> file. It tells git which files or types of files should not be
> tracked.

This is basically just a file that is saved in the same directory as the
root of the repository called `.gitignore`. It contains instructions of
which files to ignore. When creating a new repository on GitHub, you can
choose to include a gitignore file with the repository. There are
templates you can choose from which will add the most popular files you
want to exclude from tracking based on the software you are using for
your project.

The example below

``` {bash}
### GENERAL ###
data/
output/
*.csv
*.xlsx

### R ###
# History files
.Rhistory
.Rapp.history

# Session Data files
.RData
.RDataTmp

# User-specific files
.Ruserdata

# Example code in package build process
*-Ex.R

# Output files from R CMD build
/*.tar.gz

# Output files from R CMD check
/*.Rcheck/

# RStudio files
.Rproj.user/

# produced vignettes
vignettes/*.html
vignettes/*.pdf

# OAuth2 token, see https://github.com/hadley/httr/releases/tag/v0.3
.httr-oauth

# knitr and R markdown default cache directories
*_cache/
/cache/

# Temporary files created by R markdown
*.utf8.md
*.knit.md

# R Environment Variables
.Renviron

# pkgdown site
docs/

# translation temp files
po/*~

# RStudio Connect folder
rsconnect/

```
