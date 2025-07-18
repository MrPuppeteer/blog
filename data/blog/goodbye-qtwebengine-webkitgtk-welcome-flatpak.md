---
title: Goodbye qtwebengine & webkit-gtk — Welcome, flatpak!
date: '2025-07-18'
tags: ['flatpak', 'gentoo', 'linux']
draft: false
summary: So, I decided to use flatpak on my gentoo machine.
images: []
layout: PostLayout
canonicalUrl: goodbye-qtwebengine-webkit-gtk-welcome-flatpak
---

So, I decided to use flatpak on my gentoo machine. Flatpak is a utility for software deployment and package management for Linux. The reason? Compiling web engines (qtwebengine & webkit-gtk) is such a pain and unfortunately Gentoo only provide a limited binary option with specific USE flags that my system doesn't match. While for browser I can simply use the binary version unlike web engine. Using flatpak, I can eliminate compiling both of them on portage by installing packages that depends on them.

Surprisingly, I only find two packages that depends on qtwebengine and webkit-gtk, one for each. To find packages that depend on a package you can just run:

```bash
equery depends qtwebengine
equery depends webkit-gtk
```

So here is the two packages:

- calibre, an e-book management that I use to read epub books depends on qtwebengine;

- and lutris, an open-source gaming platform that I use to play PS2, PSP, and Windows games that depends on webkit-gtk.

By installing both of them on flatpak and removing them from portage, I managed to uninstall a bunch of dependencies (packages) including qtwebengine and webkit-gtk on portage.

On a side note, I noticed that protontricks, a wrapper script that I use to play Windows games on steam depends on wine. Compiling wine also takes a fair amount of time, although not as long as compiling a web engine. For wine, I usually use proton provided by steam or wine-ge provided by lutris so compiling system wine just for installing protontricks is a waste of time & resource. Fortunately, protontricks is also available in flatpak. I installed it and managed to uninstall wine on my system.

Thanks to all of this, I am now a happy flatpak user.

Anyway, my final message as the title says:

> Goodbye qtwebengine & webkit-gtk — Welcome, flatpak!
