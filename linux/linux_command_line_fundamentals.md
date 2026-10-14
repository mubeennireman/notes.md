# Linux Command-Line Fundamentals

## Organized Study Notes --- Ubuntu / WSL

These notes are organized by task so you can learn commands in a
practical order. Commands are shown for a typical Linux shell such as
Bash or Zsh.

> **Safety rule:** Practise file-changing and file-removal commands
> inside a temporary practice directory. Check the current directory
> with `pwd` before running commands that modify or delete files.

------------------------------------------------------------------------

# Chapter 1 --- Command Structure and Basic Concepts

A command commonly follows this structure:

``` bash
command [options] [arguments]
```

-   **Command:** The program or shell instruction to run.
-   **Option:** Changes how the command behaves.
-   **Argument:** The file, directory, or value the command acts on.

Example:

``` bash
ls -lh /home
```

-   `ls` --- command
-   `-lh` --- combined options: long listing and human-readable sizes
-   `/home` --- argument

Options differ between commands. Do not assume the same option means the
same thing everywhere.

------------------------------------------------------------------------

# Chapter 2 --- Find Your Location and Change Directory

## 2.1 Show the current directory: `pwd`

``` bash
pwd
```

Prints the path of the current working directory.

## 2.2 Change directory: `cd`

``` bash
cd /home
cd ..
cd ~
cd -
```

-   `cd /home` --- move to `/home`.
-   `cd ..` --- move to the parent directory.
-   `cd ~` --- move to your home directory.
-   `cd -` --- return to the previous working directory.

## 2.3 Practical example

``` bash
pwd
cd /tmp
pwd
cd -
pwd
```

This shows your starting location, moves to `/tmp`, and then returns to
the previous location.

------------------------------------------------------------------------

# Chapter 3 --- List Files and Directories

## 3.1 Basic listing: `ls`

``` bash
ls
ls /home
```

Lists entries in the current directory or in a specified directory.

## 3.2 Common options

  Command        Purpose
  -------------- ------------------------------------------------------
  `ls -a`        Include hidden entries
  `ls -l`        Long/detailed listing
  `ls -lh`       Long listing with human-readable sizes
  `ls -r`        Reverse the listing order
  `ls -t`        Sort by modification time, newest first
  `ls -tr`       Sort by modification time, oldest first
  `ls -S`        Sort by file size, largest first
  `ls -d DIR`    Show the directory entry itself
  `ls -ld DIR`   Show detailed information about the directory itself

Options can be combined:

``` bash
ls -ltr
ls -lah
```

## 3.3 Understand `ls -l`

Example:

``` text
-rw-r--r-- 1 user user 2048 Oct 10 09:30 notes.txt
```

The columns normally show:

1.  File type and permissions
2.  Hard-link count
3.  Owner
4.  Group
5.  Size in bytes
6.  Modification date and time
7.  Name

File-type characters at the start:

-   `-` --- regular file
-   `d` --- directory
-   `l` --- symbolic link

------------------------------------------------------------------------

# Chapter 4 --- Create Directories

## 4.1 Create one directory: `mkdir`

Syntax:

``` bash
mkdir [OPTIONS] DIRECTORY...
```

Example:

``` bash
mkdir projects
mkdir notes
```

Creates directories in the current directory.

## 4.2 Create nested directories: `mkdir -p`

``` bash
mkdir -p projects/web/html
```

Creates missing parent directories as needed.

## 4.3 Other useful options

``` bash
mkdir -v projects
mkdir -pv projects/web/css
mkdir -m 755 public
```

-   `-v` --- print a message for each directory created.
-   `-p` --- create missing parent directories.
-   `-m 755` --- request the initial permission mode `755`; the
    effective mode can be affected by the shell's `umask`.

Inspect a directory entry:

``` bash
ls -ld projects
```

------------------------------------------------------------------------

# Chapter 5 --- Create Files and Work with Timestamps

## 5.1 Create an empty file: `touch`

Syntax:

``` bash
touch [OPTIONS] FILE...
```

Examples:

``` bash
touch notes.txt
touch file1.txt file2.txt
```

If the file does not exist, `touch` creates it. If it exists, `touch`
updates timestamps.

## 5.2 Brace expansion

``` bash
touch file{1..5}.txt
```

Creates `file1.txt`, `file2.txt`, `file3.txt`, `file4.txt`, and
`file5.txt`.

To see the names without creating files:

``` bash
echo file{1..5}.txt
```

Brace expansion generates text; it is different from wildcard matching.

## 5.3 `touch` options

  -----------------------------------------------------------------------
  Command                             Purpose
  ----------------------------------- -----------------------------------
  `touch -a FILE`                     Update access time

  `touch -m FILE`                     Update modification time

  `touch -c FILE`                     Do not create a missing file

  `touch -d "DATE" FILE`              Set timestamps from a date string

  `touch -r REF TARGET`               Copy timestamps from a reference
                                      file to a target
  -----------------------------------------------------------------------

Examples:

``` bash
touch -a notes.txt
touch -m notes.txt
touch -c missing.txt
touch -d "2026-06-23 12:30:00" notes.txt
touch -r reference.txt target.txt
```

## 5.4 Understand timestamps

``` bash
stat notes.txt
ls -l --time=atime notes.txt
ls -l --time=mtime notes.txt
ls -l --time=ctime notes.txt
```

-   **Access time (`atime`):** When a file was last accessed, subject to
    filesystem settings.
-   **Modification time (`mtime`):** When file contents were last
    modified.
-   **Change time (`ctime`):** When file contents or metadata last
    changed. It is not the creation time.

------------------------------------------------------------------------

# Chapter 6 --- Wildcards and Filename Patterns

Wildcards are usually expanded by the shell before the command runs.

  Pattern   Meaning                      Example
  --------- ---------------------------- --------------------
  `*`       Zero or more characters      `ls *.log`
  `?`       Exactly one character        `ls name?.log`
  `[abc]`   One character from the set   `ls file[123].txt`
  `[a-z]`   One character in the range   `ls [a-z]*.txt`

Examples:

``` bash
ls *.log
ls name?.log
ls file[123].txt
cp *.txt backup/
```

Patterns generally match entries in the current directory unless a path
is included. Hidden filenames beginning with `.` usually require a
pattern that starts with `.`.

------------------------------------------------------------------------

# Chapter 7 --- Copy Files and Directories

## 7.1 Copy a file: `cp`

Syntax:

``` bash
cp [OPTIONS] SOURCE DESTINATION
```

Examples:

``` bash
cp notes.txt notes-copy.txt
cp notes.txt backup/
```

The first example creates a copy with a new name. The second copies the
file into the `backup` directory, which must already exist.

## 7.2 Copy a directory

``` bash
cp -r projects/ projects-backup/
```

`-r` or `-R` copies a directory tree recursively.

## 7.3 Common `cp` options

  -----------------------------------------------------------------------
  Option                              Purpose
  ----------------------------------- -----------------------------------
  `-r` / `-R`                         Copy directories recursively

  `-a`                                Archive mode; recursive copy with
                                      preservation of attributes where
                                      possible

  `-i`                                Ask before overwriting

  `-n`                                Do not overwrite existing files

  `-p`                                Preserve selected attributes,
                                      including mode and timestamps

  `-u`                                Copy when the source is newer or
                                      the destination is missing

  `-f`                                Force copying; may remove a
                                      destination file if necessary

  `-v`                                Show each copy operation
  -----------------------------------------------------------------------

Examples:

``` bash
cp -iv notes.txt backup.txt
cp -uv source.txt destination.txt
cp -av projects/ projects-backup/
cp -v *.txt backup/
```

Be careful when using `-f`; it can overwrite existing data.

------------------------------------------------------------------------

# Chapter 8 --- Move and Rename Files

## 8.1 Rename a file: `mv`

Syntax:

``` bash
mv [OPTIONS] SOURCE DESTINATION
```

Example:

``` bash
mv old-name.txt new-name.txt
```

This renames the file in the same directory.

## 8.2 Move a file to another directory

``` bash
mv notes.txt documents/
```

Moves `notes.txt` into the existing `documents` directory.

## 8.3 Move and rename in one command

``` bash
mv notes.txt documents/lecture-notes.txt
```

Moves the file into `documents` and gives it a new name.

## 8.4 Common `mv` options

  -----------------------------------------------------------------------
  Option                              Purpose
  ----------------------------------- -----------------------------------
  `-i`                                Ask before overwriting

  `-n`                                Do not overwrite an existing
                                      destination

  `-b`                                Back up a destination before
                                      overwriting

  `-v`                                Show each move operation

  `-t DIR`                            Specify the destination directory
                                      before the source arguments
  -----------------------------------------------------------------------

Examples:

``` bash
mv -i draft.txt final.txt
mv -v file1.txt documents/
mv -t backup/ file1.txt file2.txt
```

`rv` is not a standard Linux rename command. Use `mv`.

------------------------------------------------------------------------

# Chapter 9 --- Remove Files and Directories

## 9.1 Remove a file: `rm`

``` bash
rm notes.txt
```

Removes the file. It usually does not send the file to a recycle bin.

## 9.2 Common options

  Command        Purpose
  -------------- -------------------------------------------------
  `rm -i FILE`   Ask before removing
  `rm -r DIR`    Remove a directory and its contents recursively
  `rm -f FILE`   Force removal; do not prompt for missing files
  `rm -rf DIR`   Force recursive removal

Examples:

``` bash
rm -i old-notes.txt
rm -r old-project/
```

**Safety:** Avoid running `rm -rf` on paths you have not checked. Use
`pwd` and `ls` first, especially when practising.

------------------------------------------------------------------------

# Chapter 10 --- Identify and Compare Files

## 10.1 Identify a file: `file`

``` bash
file notes.txt
file -i notes.txt
file -b notes.txt
file -z archive.gz
file -L shortcut
```

-   `file FILE` --- identify the likely file type.
-   `-i` --- show MIME type and encoding information.
-   `-b` --- omit the filename from the output.
-   `-z` --- try to inspect the contents of a compressed file.
-   `-L` --- follow a symbolic link and inspect its target.

File extensions do not guarantee the actual file type.

## 10.2 Compare files: `cmp`

``` bash
cmp file1.txt file2.txt
cmp -s file1.txt file2.txt
```

`cmp` compares files byte by byte. It normally reports the first
difference. With `-s`, it suppresses normal output; check the exit
status to determine whether the files differ.

------------------------------------------------------------------------

# Chapter 11 --- Display and Read File Contents

## 11.1 Display content: `cat`

``` bash
cat notes.txt
cat file1.txt file2.txt
cat file1.txt file2.txt > combined.txt
```

`cat` displays files and concatenates their contents.

Useful options:

  Option   Purpose
  -------- ---------------------------------------------------------------
  `-n`     Number all output lines
  `-b`     Number non-empty lines only
  `-s`     Squeeze repeated blank lines
  `-A`     Show non-printing characters, including tabs and line endings

Examples:

``` bash
cat -n notes.txt
cat -A notes.txt
```

## 11.2 Show the beginning: `head`

``` bash
head notes.txt
head -n 20 notes.txt
head -c 100 notes.txt
```

-   Default: first 10 lines.
-   `-n 20`: first 20 lines.
-   `-c 100`: first 100 bytes.

## 11.3 Read large files: `less`

``` bash
less notes.txt
less -N notes.txt
less +G notes.txt
less +F application.log
```

Useful keys inside `less`:

  Key                Action
  ------------------ ---------------------------------------
  Up / Down arrows   Move one line
  `Space`            Move forward one screen
  `b`                Move backward one screen
  `g`                Go to the beginning
  `G`                Go to the end
  `u` / `d`          Move up/down half a screen
  `/word`            Search forward
  `?word`            Search backward
  `n` / `N`          Next match / previous-direction match
  `h`                Open help
  `q`                Quit
  `Ctrl+C`           Stop follow mode when using `+F`

`-N` enables line numbers. `+G` starts at the end. `+F` follows appended
content, similar to `tail -f`.

------------------------------------------------------------------------

# Chapter 12 --- File Permissions and Long Listings

Use:

``` bash
ls -l
ls -ld documents
stat notes.txt
```

A long listing includes file type and permissions, hard-link count,
owner, group, size, modification time, and name.

Example:

``` text
-rwxr-xr-- 1 user developers 1200 Oct 10 09:30 script.sh
```

The permission characters are grouped as:

-   **Owner:** `rwx`
-   **Group:** `r-x`
-   **Others:** `r--`

Meanings:

-   `r` --- read
-   `w` --- write
-   `x` --- execute (for a directory, it also controls traversal)

Numeric permissions:

  Number   Permissions
  -------- -------------
  `7`      `rwx`
  `6`      `rw-`
  `5`      `r-x`
  `4`      `r--`
  `0`      `---`

Example: `755` means owner `rwx`, group `r-x`, and others `r-x`.

Set permissions with `chmod`, for example:

``` bash
chmod 755 script.sh
chmod u+x script.sh
```

`chmod` changes permissions; `mkdir -m 755 DIR` requests a permission
mode when creating a directory.

------------------------------------------------------------------------

# Chapter 13 --- Command History and Terminal Shortcuts

## 13.1 History commands

``` bash
history
history | tail -n 20
history -d 25
history -w
history -c
```

-   `history` --- show recorded commands.
-   `history -d OFFSET` --- delete an entry by its history position.
-   `history -w` --- write the current history list to the configured
    history file.
-   `history -c` --- clear the current in-memory history list.

History behaviour can differ between Bash and Zsh.

## 13.2 History shortcuts

  Shortcut / expression   Purpose
  ----------------------- -------------------------------------------------
  `!!`                    Repeat the previous command
  `!25`                   Run history entry 25
  `!git`                  Run the most recent command starting with `git`
  `Ctrl+R`                Search command history incrementally
  `Ctrl+L`                Clear/redraw the terminal display

Be careful with history expansion: inspect a recalled command before
running destructive operations.

------------------------------------------------------------------------

# Chapter 14 --- Find Out What a Command Does

## 14.1 `type`

``` bash
type ls
type -a ls
```

-   `type NAME` --- show how the shell interprets the name.
-   `type -a NAME` --- show all matching definitions or locations.

## 14.2 `command`

``` bash
command -v ls
command -V ls
```

-   `command -v` --- show the resolved command name or path.
-   `command -V` --- give a more descriptive explanation.

These are useful for distinguishing aliases, functions, shell builtins,
and executable programs.

## 14.3 Help options

``` bash
help cd
ls --help
cp --help
```

`help` is commonly used for shell builtins in Bash. Many executable
programs support `--help`, but not all do.

------------------------------------------------------------------------

# Chapter 15 --- Manual Pages and Topic Search

## 15.1 Read a manual: `man`

``` bash
man ls
man cp
man mkdir
```

Inside a manual page:

-   `Down Arrow` --- move down one line.
-   `Space` --- move forward one screen.
-   `b` --- move backward one screen.
-   `/sort` --- search forward for `sort`.
-   `n` --- go to the next match.
-   `q` --- quit.

## 15.2 One-line manual description

``` bash
man -f ls
```

Displays a brief description of a matching manual entry, when available.

## 15.3 Search manual descriptions: `apropos`

``` bash
apropos password
apropos password | head -n 15
apropos file | grep -i -m 5 'copy'
```

`apropos` searches the manual-page database for matching topics. Results
depend on the available and indexed manual pages.

## 15.4 Save a short manual excerpt

``` bash
man -P cat ls | col -b | head -n 20 > ls-man-excerpt.txt
```

This extracts the first 20 lines of the `ls` manual page into a text
file after removing terminal formatting.

------------------------------------------------------------------------

# Chapter 16 --- Other Useful Terminal Utilities

## 16.1 `expr` --- evaluate simple expressions

``` bash
expr 5 + 3
expr 10 - 4
expr 6 \* 7
expr 20 / 5
```

Example output:

``` text
8
6
42
4
```

The multiplication symbol must be escaped because `*` has a special
meaning to the shell.

In Bash or Zsh, arithmetic expansion is often simpler:

``` bash
echo $((5 + 3))
```

## 16.2 `figlet` --- ASCII text art

``` bash
figlet Hello
figlet -f slant Hello
```

`figlet` displays text using large characters. The `slant` font must be
available.

Install it on Ubuntu if needed:

``` bash
sudo apt update
sudo apt install figlet
```

## 16.3 `clear` --- clear the terminal display

``` bash
clear
```

Shortcut: `Ctrl+L` in common terminal environments. Clearing the display
does not delete files or erase command history.

------------------------------------------------------------------------

# Chapter 17 --- Practical Revision Exercise

Use a dedicated practice directory so your experiments do not affect
important files.

## Step 1: Create a practice area

``` bash
mkdir -p ~/linux-practice
cd ~/linux-practice
pwd
```

## Step 2: Create files and folders

``` bash
mkdir documents backup
touch notes.txt draft.txt
touch file{1..3}.log
```

## Step 3: Inspect the results

``` bash
ls
ls -lah
ls -ltr
file notes.txt
stat notes.txt
```

## Step 4: Write and read sample content

``` bash
echo "Linux practice notes" > notes.txt
cat -n notes.txt
head notes.txt
less notes.txt
```

Press `q` to leave `less`.

## Step 5: Copy and rename

``` bash
cp notes.txt backup/
cp -v draft.txt documents/
mv draft.txt final-draft.txt
mv final-draft.txt documents/
```

## Step 6: Compare files

``` bash
cp notes.txt notes-copy.txt
cmp notes.txt notes-copy.txt
```

No output from `cmp` normally means the files match.

## Step 7: Inspect history and help

``` bash
history | tail -n 20
type -a ls
ls --help
man ls
```

## Step 8: Remove only the test files you created

``` bash
rm -i notes-copy.txt
```

Inspect the directory before removing anything else:

``` bash
pwd
ls -lah
```

------------------------------------------------------------------------

# Quick Reference

  Task                              Command
  --------------------------------- ---------------------
  Show current directory            `pwd`
  Change directory                  `cd DIR`
  Go to parent directory            `cd ..`
  List entries                      `ls`
  Show hidden entries               `ls -a`
  Detailed listing                  `ls -lh`
  Create directory                  `mkdir DIR`
  Create nested directories         `mkdir -p a/b`
  Create file / update timestamps   `touch FILE`
  Copy file                         `cp SOURCE DEST`
  Copy directory                    `cp -r SOURCE DEST`
  Move or rename                    `mv SOURCE DEST`
  Remove file                       `rm FILE`
  Remove directory tree             `rm -r DIR`
  Identify file type                `file FILE`
  Compare files                     `cmp FILE1 FILE2`
  Display file                      `cat FILE`
  Show first lines                  `head FILE`
  Read interactively                `less FILE`
  Show file metadata                `stat FILE`
  Display history                   `history`
  Search manual topics              `apropos TOPIC`
  Open manual                       `man COMMAND`
  Clear terminal display            `clear`

------------------------------------------------------------------------

## How to Study These Notes

For each chapter:

1.  Read the purpose of each command.
2.  Type the examples yourself in the practice directory.
3.  Change the filenames and arguments to see what changes.
4.  Close the notes and try to recall the syntax.
5.  Revisit only the commands you cannot recall.

**Learning priority:** Navigation → listing → directory creation → file
creation → copying → moving/renaming → removal → inspecting contents →
permissions → history and help.

Keep these notes as a reference. The goal is not to memorize every
option at once, but to understand the common commands and know how to
look up the rest.
