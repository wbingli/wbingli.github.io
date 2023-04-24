---
layout: post
title: "Using Atom Editor in Ubuntu"
date: 2014-05-14 11:01:26 +1200
comments: true
categories: tool,atom
---

[Atom](atom.io) is a new editor from github.com, which is very similar to Sublime. It's written using nodejs, coffeescript and less. And it's **opensource**. It makes lots of potentials for the future of the editor.

However, atom does not provide a Linux package yet. As it's opensource, we can build from source.

## Build Atom Editor for Ubuntu 14.04

From [atom github repo](https://github.com/atom/atom) you can find the [Linux installation guide](https://github.com/atom/atom/blob/master/docs/build-instructions/linux.md). Basically just following this guide should be OK.

The only problem I met is **node-gyp rebuild** issue. It's because of the **gyp** installed in your machine which is conflicting with gyp version in node-gyp.

Simple solution is :

```bash
sudo apt-get remove gyp
```

After that, you will find a .deb atom file under */tmp/atom-build* folder. Then install it :

```bash
cd /tmp/atom-build
sudo dpkg -i atom-0.95.0-amd64.deb
```

Now you can open it by command `atom`.

## Configure keybindings for Ubuntu
Most of the keybindings in atom are for Mac OS. You can add your own keys in key map file `~/.atom/keymap.cson`. There are some basic hotkeys I have added for Ubuntu:

```
'.editor':
    'ctrl-l': 'editor:select-line'
    'ctrl-d': 'find-and-replace:select-next'

'body':
  'ctrl-pagedown': 'pane:show-next-item'
  'ctrl-pageup': 'pane:show-previous-item'
```
Check the source code e.g. [edit-view.coffee](https://github.com/atom/atom/blob/master/src/editor-view.coffee) for available commands you can use.

Happy hacking!
