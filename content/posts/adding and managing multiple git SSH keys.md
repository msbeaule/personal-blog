---
title: "Adding and Managing Multiple Git SSH Keys"
date: 2023-04-12
draft: false
tags: ["git", "ssh", "github"]
comments: true
description: "Adding and managing multiple git SSH keys on Windows 10 with Git Bash and .bash_profile."

# weight: 1
# aliases: [""]
# author: ""
# showToc: true
# TocOpen: true
# hidemeta: false

# disableShare: false
# hideSummary: false
# searchHidden: true
# ShowBreadCrumbs: true
# ShowPostNavLinks: true
# ShowWordCount: true
# ShowRssButtonInSectionTermList: true
# UseHugoToc: true

---

This is how I currently manage multiple SSH keys for Github on a single computer. Thanks to this [gist](https://gist.github.com/oanhnn/80a89405ab9023894df7) for giving me the idea to look into this.

## Creating a SSH key

On Windows 10 using Git Bash, navigate to the `~/.ssh` folder, using `cd ~/.ssh`. If this folder doesn't exist, create it using `mkdir ~/.ssh`.

Github recommends (as of writing) running this command [to create a SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) (make sure to replace this example email with your own):

```shell
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```

Give a name to your key, optionally give it a password then the key will be generated.

You can add your key by first running this command to start the SSH agent:

```shell
$ eval $(ssh-agent)
```

Then add your key (the one _without_ the `.pub`) to this Git Bash session using:

```shell
$ ssh-add ~/.ssh/your_key
```

Then add the `.pub` version of the key to [Github](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

## Auto adding a SSH key when Git Bash starts

Adding a SSH key only works for that session, once you close and reopen Git Bash you'll need to add it again.

If you want the SSH key to automatically be added each time Git Bash is opened, on Windows 10 open your `.bash_profile` file. From Git Bash you can open this by running `start ~/.bash_profile`. If that doesn't work, open the folder it's in by running `start ~`.

Then add this to your `.bash_profile` and it should run each time Git Bash is opened:

```shell
eval $(ssh-agent)
ssh-add ~/.ssh/your_key # replace with your own key
```

## Managing multiple SSH keys

If you have more than one Github account with different SSH keys for each, you might instead need to pick which one you want each time Git Bash runs. In this case, you can add the SSH code inside a function in `.bash_profile`:

```shell
function ssh-add-custom {
    eval $(ssh-agent)
    ssh-add ~/.ssh/your_key # replace with your own key
}

function ssh-add-custom-two {
    eval $(ssh-agent)
    ssh-add ~/.ssh/your_key_2 # replace with your own key
}
```

In the above example, you can add either SSH key to your current session by typing `ssh-add-custom` or `ssh-add-custom-two` in Git Bash, depending on which you need.