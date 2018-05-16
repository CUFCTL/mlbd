## Git

Git is a _version control system_, or an application which allows you to keep track of changes you make to a project over time. That way, you can always see what code is finished versus what code you're experimenting with, and if you make a mistake at some point you can go back to a previous version. Github is a site that hosts Git repositories; there are other sites that do the same thing, like GitLab and Bitbucket. Almost every piece of software that we use in the lab is managed as a Git repo, and almost all of them are hosted on Github or a similar site.

Git is a _very_ powerful application with a lot of features, so this guide will just give you the main commands to know for everyday use. You should try to learn about Git a little at a time as you go, but in the meantime, this guide should provide everything you need. More exhaustive help can be found at https://help.github.com.

### Setup

Git is easy to install on Ubuntu:
```
sudo apt-get install git
```

With git installed, configure your username:
```
git config --global user.name [name]
git config --global user.email [email]
```

You can download a git repository to your local machine by "cloning" it from Github. For example, to get started with the `face-recognition` project:
```
git clone https://github.com/CUFCTL/face-recognition.git
```

### Developing

As you make changes in your workspace, you can track them with `git status`.

You can also use `git diff` to show a diff of the files you have changed (use `git diff --cached` for files that are staged).

When you're ready to commit your changes to your local repo, use `git add [files]` to stage the changed files and `git commit -m [message]` to commit them. You have to have a message for every commit. Use `git status` when you add files to make sure they are staged.

If the files you changed are already being tracked, you can use `git commit -am [message]` to stage and commit at the same time.

If you screw up a commit, use `git reset HEAD^` to undo the commit (you can add more `^`'s to undo multiple commits). Your files will be the same, just with the uncommitted changes. Try to catch mistakes before you push them to the main repo, because it's a pain to undo them after that!

Try to make your commits clean and modular, e.g. make changes to add one feature or fix one bug, make sure the changes work, then commit them. That will make your commit message easy to write. Don't just change a bunch of stuff all over the place in one commit! That will make it hard for people in the future to figure out what you changed.

### Pulling and pushing

When you join the project, you will be given push rights to this repository. That means you have to be careful about pushing your changes, because everyone else can push their changes too, and if we don't push responsibly then there will be conflicts. With that in mind, try to stick to the following work-flow:

1. Before you develop, pull any new commits from Github:
```
git pull
```
2. Make sure no one else is working on the same code as you, but if you are working with someone else on the same code, you should probably coordinate in person.
3. Develop!
4. Before you commit your changes, you might want to `pull` again just in case someone pushed more commits while you were working.
5. Push your commits to Github:
```
git push
```

### In Case of Emergency

If you commit new changes and then try to pull, git won't let you pull! And if you try to push but the main repo has new commits that you didn't pull, git won't let you push! At least not with the commands in this guide, so don't try to force git in these situations because you'll make a mess! Follow these steps instead:

1. Undo your commits with `git reset HEAD^` until you're on the same page as the main repo.
2. Pull
3. Commit your changes
4. Push

### Branching

Sometimes you might want to experiment with a new feature, but not add it to the project yet. Maybe other people are working on the same feature so you need to collaborate. In this case, you can create a separate branch to version control your experimental feature. The main repo that you commit to is itself a branch called `master`; to make a new branch and use it, follow these steps:

1. Create the branch and switch to it:
```
git checkout -b [branch-name]
```
2. Develop, commit changes to your branch with `git commit`.
3. Push your branch to Github with `git push`.

### Stashing

Sometimes you might want to experiment with a new feature, but not add it to the project yet. So you read the section above on branching, but it seems like too much work. Instead of branching, you can just use stashing to save your changes for later while you work on something else:

1. Stash your changes:
```
git stash
```
2. Work on something else, commit changes, etc.
3. Grab your changes from out of the stash:
```
git stash pop
```

No branching necessary! However, if you're stashed changes conflict with the other changes you made, you will have to go through the changes and merge them manually.

### GUI Tools

[Atom](https://atom.io) has a Git plugin which makes using Git much easier. There are also GUI clients for Git / Github, but it is recommended that you don't use these tools unless you are very experienced with git. Otherwise, you will very likely cause problems down the road.
