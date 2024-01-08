---
title: "Technic Launcher not opening? Here's two possible fixes"
date: 2023-08-06T12:15:00
draft: false
tags: ["minecraft", "technic launcher"]
categories: ["Troubleshooting"]
description: "Uninstalling then reinstalling Java should work, most of the time."
# lastmod: 

uniqueHash: "8820064123346663"

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

## The problem

Recently I used Samsung Migrate to clone my C drive and Windows 10 on one SSD to another.

Then I tried to open the Technic Launcher for Minecraft and it wasn't opening. The logo appeared on my desktop for a few seconds then it would disappear.

## The error logs

Looking at [Event Viewer](https://en.wikipedia.org/wiki/Event_Viewer) under Windows Logs > Application, I saw these error logs:

> Fault bucket , type 0
> Event Name: APPCRASH
> Response: Not available
> Cab Id: 0
> 
> Problem signature:
> P1: TechnicLauncher.exe
> P2: 4.0.0.803
> P3: 64b1d71a
> P4: msvcrt.dll
> P5: 7.0.19041.546
> P6: 7f567a50
> P7: c0000005
> P8: 00086870
> P9: 
> P10: 

And this:

> Fault bucket , type 0
> Event Name: APPCRASH
> Response: Not available
> Cab Id: 0
> 
> Problem signature:
> P1: TechnicLauncher.exe
> P2: 4.0.0.803
> P3: 64b1d71a
> P4: StackHash_f0ed
> P5: 0.0.0.0
> P6: 00000000
> P7: c00001a5
> P8: PCH_96_FROM_ntdll+0x0007300C
> P9: 
> P10: 

And this too:

> Fault bucket , type 0
> Event Name: BEX
> Response: Not available
> Cab Id: 0
> 
> Problem signature:
> P1: TechnicLauncher.exe
> P2: 4.0.0.803
> P3: 64b1d71a
> P4: msvcrt.dll
> P5: 7.0.19041.546
> P6: 7f567a50
> P7: 00086870
> P8: c0000409
> P9: 00000015
> P10: 

## Possible solutions

### 1. Uninstall Java then reinstall

This is the solution that worked for me. I know sometimes when doing a clone of a drive some things might not work for whatever reason.

Open your Control Panel in Windows, then under Programs click on "Uninstall a program". Search for "java", "jre", and uninstall those. I had a lot of different versions of Java so I uninstalled them all for a clean slate.

Next, reinstall Java Runtime Environment (JRE) 1.8, the 64 bit version. You can download JRE from [OpenLogic](https://www.openlogic.com/openjdk-downloads?field_java_parent_version_target_id=416&field_operating_system_target_id=436&field_architecture_target_id=391&field_java_package_target_id=401), [Adoptium](https://adoptium.net/en-GB/temurin/releases/?version=8), or [Oracle here](https://www.java.com/en/download/).

Now try running the launcher again, hopefully after a few seconds it launches. If it still doesn't work, either try the solution below, or try searching for and uninstalling "jdk" in Control Panel as well. Restart your computer and try running the launcher again.

### 2. Check the name of the folder

If your Technic Launcher app is in a folder that has an exclamation mark `!` in it, or possibly any other non alphanumeric characters, it might refuse to open. Either move the launcher to another folder or remove the special characters.

Source: [YouTube](https://www.youtube.com/watch?v=cO0TdxZczjk)
