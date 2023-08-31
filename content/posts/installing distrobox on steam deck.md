---
title: "How to install command line programs on the Steam Deck's immutable OS"
date: 2023-08-31T16:12:00
draft: false
tags: ["steam deck", "distrobox"]
categories: ["how to"]
description: "Installing Distrobox is very simple and gives you access to many Linux distros right in your terminal, and all the apps you can normally install on them."
# lastmod: 

# comments: true
# weight: 1
# aliases: ["/first"]
# author: ""
# showToc: true
TocOpen: true
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

The Steam Deck OS is immutable, so by default you can't install anything outside of the user partition. If you really want to, there's a [command you can run](https://help.steampowered.com/en/faqs/view/671A-4453-E8D2-323C) to install things outside of it but when there's an OS update, it will all be wiped.

This is where Distrobox comes in. You can install it in the user directory, and it won't be removed when there's an OS update. There's also the added bonus of being able to install almost any Linux distro you'd like. If you've ever used the [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/about), this is the same idea.

## Set a root password

Start with setting a root password in Konsole if you haven't already. Run the following command and follow the instructions in the terminal:

```bash
passwd
```

## Download Distrobox and Podman

We need to install Distrobox and [Podman statically](https://github.com/89luca89/distrobox/blob/main/docs/compatibility.md#install-podman-in-a-static-manner). Use these following commands to install both to your `~/.local` directory:

```bash
curl -s https://raw.githubusercontent.com/89luca89/distrobox/main/install | sh -s -- --prefix ~/.local
```

```bash
curl -s https://raw.githubusercontent.com/89luca89/distrobox/main/extras/install-podman | sh -s -- --prefix ~/.local
```

Now add both of these to your path so you can easily use them. First open your `~/.bashrc`:

```bash
nano ~/.bashrc
```

Then add these two lines to the bottom of it, then save it:

```
export PATH=$PATH:$HOME/.local/bin
export PATH=$PATH:$HOME/.local/podman/bin
```

If you want to run an app with a UI, add this to your `~/.bashrc` too:

```
xhost +si:localuser:$USER
```

## Choosing a Linux distro

View all the [Linux distro images here](https://github.com/89luca89/distrobox/blob/main/docs/compatibility.md#containers-distros), under the "Containers Distros" heading.

## Creating a distro image

For example, if you want to install Arch Linux, you would run this:

```bash
distrobox create --image archlinux
```

And for Debian 12:

```bash
distrobox create --image debian:12
```

And then follow the instructions in the terminal.

For more details on `distrobox create`, [view the docs here](https://github.com/89luca89/distrobox/blob/main/docs/usage/distrobox-create.md).

## Entering the Linux distro

To see all the distros that you've installed, and their names that you need to run the next command with, use:

```bash
distrobox list
```

Once the distro of your choice is created, you can enter it using:

```bash
distrobox enter <distro-image-name>
```

For example, we'd use this to enter the Debian 12 distro we made above:

```bash
distrobox enter debian-12
```

Now you should see your terminal slightly change, instead of seeing something like this:
`(deck@SteamDeck ~)$`, you should see something like this: `deck@debian-12:`.

## Exiting a distro

Once inside a distro image, if you want to exit it but have it still running:

```bash
exit
```

## Stopping a distro

If you want to stop a distro image, run this command with the same name that you entered it with:

```bash
distrobox stop <distro-image-name>
```
