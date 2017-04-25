+++
date = "2017-04-25T10:45:23+01:00"
title = "Terminal 101 - The Basics"
+++

Most developers out there prefer to use a Command Line Interface (CLI) over the
usual Graphical User Interface (GUI) for various reasons, the most common ones
being:

* It's *(usually)* faster to use the CLI rather than the GUI;
* You don't need to move your hands away from the keyboard (to reach the mouse);
* You can combine multiple CLI commands;
* When working on a remote server, we often don't have access to a GUI;

It does seem scary and challenging the first time you look at it, but as soon as
you get the gist, you'll see it's not much different than using the normal
interface. 

Let's start by defining what a command actually is. A **command** is nothing
more than an instruction given by the user telling the machine to do something,
such as listing the files inside a directory or deleting a directory. Commands
are (most of the times) issued by typing them in at the command line (the
terminal) and then pressing the <kbd>↵</kbd> (enter) key, which passes the
command to the shell.

**Shell** is the program that receives the command and actually executes it. On
most Linux systems (including on Mac OSX), the default shell is the *bash* shell.

Most commands also accept a pre-defined set of options, known as **flags**, that
can be passed in to change the default behaviour of the command. A flag is
usually a single letter or words prefixed by a dash (`-`). For example, the `rm`
command is used to remove a file and, by default, doesn't ask for user
confirmation; if used in conjunction with the `-i`, it will ask for confirmation
before actually deleting the file.

With these concepts defined, it's time to finally open your terminal!

## Choosing a Terminal

Unix machines usually come with a pre-installed terminal software. On Mac OSX,
You can open it by pressing <kbd>⌘</kbd>+<kbd>space</kbd> (on Linux,
<kbd>Alt</kbd>+<kbd>F2</kbd>), typing "Terminal" on the box followed by
<kbd>↵</kbd>.

> TIP: The default terminal on OSX is quite simplistic. I recommend using
> [ITerm2](https://www.iterm2.com/downloads.html) instead.

With your terminal open, it's time to start firing some commands!

## Basic Commands

> TIP: To read the man page of a command, try typing `man command`+<kbd>↵</kbd>. For example: `man ls`.

### `ls` - List Directory Contents

Syntax: `ls [target]`.

> TIP: when you see square brackets in a command syntax description (such as in
> the `ls [target]` above), it means that [target] is optional.

To list the contents of a directory, type `ls` + <kbd>↵</kbd> in your Terminal
window (from now on, I'll omit the enter key after the command). You should see
the files that exist in your **HOME** directory (more on HOME later). To see a
detailed list, you can execute `ls` with the `-l` flag, that is, `ls -l`.

<img class="screenshot" alt="ls command" src="/img/terminal/ls.png" />

You can also specify a directory after `ls`, to get the list of contents of the
directory (for example, `ls Downloads` will show the files inside the Downloads
directory).

### `cd` - Change Directory

Syntax: `cd target`.

To change directories, type `cd` followed by the name of the directory you want
"cd" in, for example, `cd Downloads`.

> TIP: Use the <kbd>TAB</kbd> to auto-complete the name of the directory; For
> example, if you type "cd Down" and press <kbd>TAB</kbd>, the shell will
> auto-complete to "cd Downloads/"

When you open a new terminal window, you're (usually) in the HOME directory. You
can go back to HOME at anytime by typing `cd $HOME` or `cd ~`. To go back to the
previous directory you were in, use `cd -`. You can go up one directory by using
`..` as the directory name. 

It is possible to combine directories and change to a subdirectory directly. For
example, if you have the following directory tree:

```
HOME/
└── dev/
    └── onionworks/
        ├──foo/
        └── bar/
```

You could navigate the tree with the following commands:

```bash
cd dev/onionworks/foo   # HOME/dev/onionworks/foo
cd ..                   # HOME/dev/onionworks
cd bar                  # HOME/dev/onionworks/bar 
cd ../../               # HOME/dev
cd ~                    # HOME
```

<img class="screenshot" alt="cd command" src="/img/terminal/cd.png" />

### `mkdir` - Make Directory

Syntax: `mkdir name`.

To create new directories, type `mkdir` followed by the name of the directory
you want to create, for example, `mkdir newdir`. You can use the `-p` flag to
create multiple subdirectories at the same time.

<img class="screenshot" alt="mkdir command" src="/img/terminal/mkdir.png" />

### `cp` - Copy Files

Syntax: `cp source destination`.

To copy files around, type `cp` followed by the file you want to copy and the
place you want the file to be, for example, `cp Downloads/image.png Pictures/`.
If the last argument is a directory (like the example), the file will be copied
to that directory; otherwise, a new file with the provided name will be created.

```bash
cp image.png Pictures/      # will copy to Pictures/image.png
cp image.png new_image.png  # will copy to new_image.png
```

You can copy multiple files using the `*` symbol:

```bash
cp Pictures/* Downloads/
```

By default, `cp` will now copy subdirectories. To include subdirectories, you
can use the `-r` flag:

```
ls Pictures/
> image.png folder/ 
cp -r Pictures/* Downloads/
ls Downloads
> image.png folder/
```

<img class="screenshot" alt="cp command" src="/img/terminal/cp.png" />

### `mv` - Move Files

Syntax: `mv source destination`.

While `cp` copies the file -- i.e., creates a new file into the destination that
has the same contents as the file on source, `mv` will move the file (or
directory) from source into destination -- i.e., the original source file will
no longer exist after a successful `mv`.

> TIP: you should use `mv` if you want to rename an existing file

<img class="screenshot" alt="mv command" src="/img/terminal/mv.png" />

### `rm` - Remove Files

Syntax: `rm target`.

You can delete files using `rm`. Note that, by using `rm`, the file will be
deleted and forever gone -- it **will not** be moved to a "Trash bin" where you
can later restore.

By default, `rm` will remove files only. Similarly to `cp`, you can use `*` to
match multiple files. For example, to remove all png files from the Downloads
directory, one can type: `rm Downloads/*.png`.

If you want to be prompted to confirm the deletion, add the `-i` flag. To remove
all files, including subdirectories and its contents, use the `-r` flag.

<img class="screenshot" alt="rm command" src="/img/terminal/rm.png" />

# Conclusion 

It takes some time to get used, but you will soon find that it's much faster to
use the CLI than the GUI, but don't worry! It does get easier from here and will
soon retire your Finder/Explorer window in favour of a terminal. When you get
comfortable navigating with the CLI, ensure to explore the man pages of the
commands -- each command has a lot more flags than the ones explained here.
Check back soon for more terminal tips!

---

Resources:

* http://www.linfo.org/command.html

<!-- *This post is also available in [Portuguese](/pt/blog/terminal-basics)* -->
