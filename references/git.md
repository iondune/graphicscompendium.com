---
layout: page
active: references
title: "Using Git"
auto-title: true
---

Git is not a graphics topic, but it is a software engineering topic.
If you already know and use Git, then feel free to skip to the next chapter.

I won't try to teach you the fundamentals of Git.
There are approximately 1,000,000 tools that can do a better job of that.
So far as I know, [this is as good as any](https://try.github.io/).
If you have any alternatives to recommend, please let me know!

Instead, this chapter is about some motivation for using Git,
some best practices, and some general advice.
I'll also touch on a few topics that I think are commonly confusing for early Git users.


## Creation

There are two different ways to create a git repository.
One way is by cloning an existing repository (e.g. from GitHub),
and the other is using the `init` command.
Create a git repository by running this command in a directory with files you want to put in a repository:

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
These files should never be checked into a repository.

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
You will need to copy either the **HTTPS** or **SSH** clone url for your repository.

![clone or download](git-figure-clone.png)

**HTTPS** will require your GitHub password every time you try to push.

**SSH** will require that you set up [SSH Keys](https://help.github.com/articles/connecting-to-github-with-ssh/)
with your GitHub account, but will never ask for a password when you push!

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
