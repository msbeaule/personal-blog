---
title: "Delete Files in Windows Recycle Bin from the Command Line"
date: 2023-04-18T01:05:00
draft: false
tags: ["windows", "command prompt", "delete files"]
categories: ["how to"]
description: "How to delete files in the recycle bin from the Windows command prompt, and how to quickly and permanently delete thousands of files using the command prompt before it becomes a problem."
# lastmod:

# comments: true
# weight: 1
# aliases: ["/first"]
# author: ""
# showToc: true
# TocOpen: false
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

I had a problem a while back where I had roughly 600k+ files in a folder inside my temp folder (I was running a video downloader and it downloaded videos in parts, and after it merged the parts together it never deleted the thousands of temp files) in Windows 10, and I wanted to clear them all out. The recycle bin couldn't handle anywhere near that many files, so I wasn't able to use the recycle bin's GUI to empty it. It was taking forever to load, and my RAM usage was increasing a lot. This is a known bug _([2018 post](https://answers.microsoft.com/en-us/windows/forum/all/recycle-bin-memory-leak-windows-10/06309475-dc70-4acc-8953-4bab7fc472e1), [2022 post](https://answers.microsoft.com/en-us/windows/forum/all/recycle-bin-extreme-memory-usage-resulting-in-os/37e7c09b-21bf-40ad-927c-8d8150fd6b52))_, but fortunately there's a way around this.

## Delete files inside the recycle bin using command prompt

Run Command Prompt as administrator, then run this command. This is assuming your recycle bin is on your C drive, if it's located somewhere else replace `C` with the drive letter it's on.

(**Note**: this command doesn't work in PowerShell):

```cmd
rmdir /s /q C:\$RECYCLE.BIN
```

A breakdown of the above command: [^1] [^2]

- `/s` means delete all files and subfolders inside the specified folder (i.e. recursive)
- `/q` means quiet mode, so no prompts will be given to you when deleting everything

## Delete files permanently before they become a problem (skip the recycle bin)

Instead of moving thousands of files to the recycle bin then emptying it, you can skip the recycle bin by permanently deleting files.

First navigate to the parent folder of the folder you want to delete using `cd`:

```cmd
cd path/to/parent/folder
```

{{< notice danger >}}Triple check you typed the name of the right folder, because _everything_ inside it will be deleted very quickly. If you need to cancel running a command before too much damage is done, press `ctrl` + `c` multiple times while the command prompt running the command has focus / is selected.{{< /notice >}}

Then run these commands inside the parent folder:

```cmd
del /f /q /s FOLDERNAME > nul
```

```cmd
rmdir /s /q FOLDERNAME
```

A breakdown of the first command, `del`: [^3] [^4]

- `/f` means force delete all files, even the read-only ones
- `/q` means quiet mode, so no confirmation prompts will be given to you
- `/s` means delete all files from the specified folder and from all subfolders (i.e. recursive)
- `FOLDERNAME` replace this with the folder you want all files to be deleted in (all files in subfolders also get deleted)
- `> nul` is so all the deleted files aren't displayed in the command prompt, so the command runs slightly faster and cleaner (especially when deleting thousands of files). If you want to see the output of everything being deleted, run the command without it: `del /f /q /s FOLDERNAME`

A breakdown of the second command, `rmdir`:

- `/s` means delete all files and subfolders inside the specified folder (i.e. recursive)
- `/q` means quiet mode, so no prompts will be given to you when deleting everything
- `FOLDERNAME` replace this with the folder you want all subfolders and itself to be deleted

{{< notice info >}}The `rmdir` command above can also remove files (not just folders), but it's much slower so that's why we run the `del` command first.{{< /notice >}}

Essentially, the first command (`del`) deletes all the files (from the specified folder and all subfolders), while the second command (`rmdir`) deletes all the subfolders and then the specified folder. So you don't have to run the second command if there's no subfolders.

This deleted everything inside my temp folder pretty fast. It took maybe a minute or two to delete ~600k files on my SSD.


[^1]: [Can't clear Recycle Bin, too many files](https://superuser.com/questions/1038602/cant-clear-recycle-bin-too-many-files)
[^2]: [`rmdir` flags](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/rmdir)
[^3]: [`del` flags](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/del)
[^4]: [What's the fastest way to delete a large folder in Windows?](https://stackoverflow.com/questions/186737/whats-the-fastest-way-to-delete-a-large-folder-in-windows)