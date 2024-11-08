# Bash hotkeys and tricks

These are my most-used hotkeys and tricks with the command line.

Bash has *a lot* of functionality, but much of it is not really more efficient than using the mouse
or typing stuff manually. This is a collection of the hotkeys I've actually internalised and helps
me be (or feel) more efficient.

## Completion

Hotkeys that makes the shell do more of the typing for you.

### ++tab++

Goes without saying for many people, but it is my most used key **by far**. I probably use this
between every 5 other keystrokes on average, just for good measure. It never hurts, sometimes does
nothing, mostly helps. Single-tap it to autocomplete unambigiously, double-tap it to list
alternatives. It:

- completes file paths
- helpfully enters the slash character for you in path names
- completes environment variable names
- completes commands and command arguments (if this doesn't work, make sure you have the
  bash-completion package installed).
- hints at availably command arguments (note that it might not be 100% accurate or complete, it
  depends on the content of the bash-completion script for the specific command)

!!! tip

    It is possible to create custom completion files for your custom scripts. That is out of scope
    for this article, but you'll find good resources on it elsewhere.


### ++alt+period++

Probably my second most used combination. Inserts **the last argument from the previous command**.

This might sound obscure and a bit silly, but is surprisingly good, because of how arguments on the
command line is usually structured (e.g. filename goes last).

For example:

```bash
mkdir -p my_directory/with_the/longest-name_possible # make a nested directory
cd [alt+.] # navigate to the newly created directory
# or
ls -l myfile # list a file
cp [alt+.] [alt+.].bak # make a backup of the file
# or
vim myscript.sh # edit a file
./[alt+.] # execute the script you just edited
git add [alt+.] # stage it with version control

```

You can also tap it multiple times to get the last argument from the next most recent command, etc.

For example, for the following history:

```shell
~ $ history
(...)
1234 vim test.sh
1235 ls -l
1236 echo test1 test2
```

- First ++alt+period++ inserts `test2`
- Second ++alt+period++ inserts `-l`
- Third ++alt+period++ inserts `test.sh`
- etc.

!!! tip

    You can also use ++alt+0++ then ++alt+period++
    
    A variation of the ++alt+period++ hotkey. Will paste the 0th argument of the previous command
    (which is actually the base command itself!)

    It also works with other numbers than 0, but keeping track of which parameter has which index
    gets difficult and is error-prone, so I find it is mostly its better to copy-paste or write it
    manually at that point. `0` (first) and `.` (last) is easy to keep track of.
    
    Useful when you have a difficult-to-write command, or a command that shares the first letters
    with other commands, making Tab completion less than ideal:

    - `firewall-cmd --add-port=123/udp` then `firewall-cmd --reload`. Last command becomes:
      (`[alt+0][alt+.] --reload`)
    - `systemctl status myservice` then `systemctl restart myservice`. Last command becomes:
      (`[alt+0][alt+.] restart [alt+.]`)

    Also useful when you want to use the man page for the last command, like `man [alt+0][alt+.]`


### ++ctrl+d++

Basically runs the `exit` command **when your prompt is empty**. If you've started writing
something in the terminal, this hotkey does nothing.

It will let you:

- Exit out of your `sudo -i` shell
- exit your current SSH session
- exit your interactive Python interpreter
- exit out of your mysql client
- quit your tmux session
- and probably much more

without writing the exit command explicitly.


### ++ctrl+r++

Reverse command search.

- You know what your command is, but you don't feel like typing it all.
- You remember that you need to export a variable, but you don't remember the exact details.
- You just want to quickly re-run the last `vim` command without scrolling through the history
  with ++up++.

Just use ++ctrl+r++, then search for something unique to your command.

Didn't find it on the first try? Just run ++ctrl+r++ again for the next match.

Mistyped it? Use ++ctrl+c++ and try again.

Want to edit it? Use ++ctrl+a++ or ++ctrl+e++ when you've found it (more info on these below).


### `!!`

This basically just means "the entire last command". I've found about two uses for it, but I use
them pretty often:

`sudo !!` - Repeat the last command, but with sudo

`watch !!` - Run the last command over and over automatically to watch for changes.

!!! tip

    There are *a lot* of these "Event designators", starting with `!`, but this is the one I
    actually use. You can also use `!123` for the 123rd entry in your bash history, `!-1` for the
    latest minus 1 command, etc. The issue is that you won't know for sure what the command
    represents until you either run it, or expand it with ++esc++ ++ctrl+e++.


## Editing

Commands to make changes to the current work-in-progress command and terminal output.

### ++ctrl+l++

Clears the screen, giving you a fresh start without needing to hold ++enter++ forever.

Why not just use `clear`?

- If you're in the middle of writing something in the prompt, that will be preserved.
- Also ++ctrl+l++ is faster.

### ++ctrl+a++ and ++ctrl+e++

To move the cursor to the start (++ctrl+a++) and end (++ctrl+e++) of the current line.

### ++ctrl+u++

Cuts/removes anything to the left of the cursor.

Less used, but the opposite is ++ctrl+k++ which cuts/removes anything to the right of the cursor.

### ++ctrl+y++

Will paste whatever you've last cut with ++ctrl+u++.

A very useful combination is ++ctrl+e++ ++ctrl+u++, which removes the entire line you are working
on. Now you can run another command, maybe an `ls` command to ensure you are doing the right thing,
before pasting the other command back with ++ctrl+y++ and running it.

### ++ctrl+w++

Cuts/removes one word at the time, to the left. Note that if you run multiple ++ctrl+w++ commands
without doing something else in between, the words will be added to the paste buffer, so that when
you run ++ctrl+y++ later, all the words you deleted with ++ctrl+w++ sequentially will be pasted
back.

### ++ctrl+s++ and ++ctrl+q++

You probably used ++ctrl+s++ before, "freezing" your terminal. It can be unfrozen with ++ctrl+q++.

But it can be used intentionally too, for example if you want to highlight/copy something before
some other output scrolls it away.

Depending on your terminal emulator, multiplexer, line history, etc. you might be able to scroll
back up and get it later, but why wait when you can just pause and do it right now?


### ++alt++ ++left++ and ++right++

To jump word-by-word instead of letter-by-letter through your current command.

### ++ctrl+x++ ++ctrl+e++

Opens an editor (determined by the EDITOR environment variable), including whatever you've already
wrote on the prompt. Very useful for commands with many parameters like complex curl commands.

Save and quit to execute it.

## Directory navigation

### `cd`

The same as `cd ~`, `cd $HOME`, etc.

### `cd -`

Will navigate to the previous folder you were in. Very nice when you need to jump to another folder
temporarily, then go back.

```shell
cd ../my/other/folder
ls -l
cd - # to go back to where you were
```

### `cd ~foo`

Go the the home folder of the `foo` user
