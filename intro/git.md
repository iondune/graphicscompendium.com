---
layout: page
active: references
title: "Use Git and GitHub Classroom"
auto-title: true
---

There are approximately 1,000,000 tools that can help you learn git.
So far as I know, [this is as good as any](https://try.github.io/).
If you have any alternatives to recommend, please let me know!



## Base Code

The base code for each assignment is contained in a Git repository hosted on GitHub.
You can download the basecode as a `.zip`, or you can clone it.
If you clone the repository, you will not need to follow the "creation" steps below.



## Hand-In

We will be using [GitHub Classroom](https://classroom.github.com/) to turn in assignments this quarter.


### Creation

If you downloaded the base code (not cloned, so you don't have a repo yet) you need to create repo where your source code is.
Create a git repository by running this command in the directory with your files:

```bash
git init
```

Add your source code and readme to using the **add** command it like this:

```bash
git add src/
git add resources/
git add ext/
git add CMakeLists.txt
git add README.md
git status
git commit -m 'Add my source, build scripts, and readme!'
```

Do **not** include the `build/` directory or anything in it.

### Update

If you make changes after a commit, or if you cloned the repo instead of downloading it,
you also use the **add** followed by **commit** command to apply your changes.

```bash
git add src/main.cpp
git add README.md
git commit
```

The **status** command can be used to see what files have changed, what need to be added, etc.

```bash
git status
```

### Push

After you have committed things, you need to **push** in order to upload your changes to GitHub.

First you need to accept access to your repository by clicking on the blue button at the top of the assignment page.
After you accept the assignment, you will be able to select a **clone** url.
You will need to copy either the **HTTPS** or **SSH** clone url for your repository.

![clone or download](git-figure-clone.png)

**HTTPS** will require your GitHub password every time you try to push.

**SSH** will require that you set up [SSH Keys](https://help.github.com/articles/connecting-to-github-with-ssh/) with your GitHub account,
but will never ask for a password when you push!
I am happy to help you set this up.

Once you have picked and copied the URL for your repository,
run this command if you originally cloned the base code repository:

```bash
git remote set-url origin YOUR_COPIED_URL
git push origin master
```

Otherwise, if you `init`ed the repository instead of cloning it, do this:

```bash
git remote add origin YOUR_COPIED_URL
git push origin master
```

Then your code should be saved in GitHub Classroom!



## Troubleshooting

If your repository already has a `origin` remote (and you get some sort of error about not being able to add the origin remote),
run this command instead:

```bash
git remote set-url origin YOUR_COPIED_URL
```
