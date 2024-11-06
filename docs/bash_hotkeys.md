# Bash hotkeys and tricks

These are my most-used hotkeys and tricks in the command line

## Completion

++tab++

The most used key by far. I probably use this between every 5 other keystrokes on average, just for
good measure. It never hurts, sometimes does nothing, mostly helps.

- Completes file paths
- Completes commands and command arguments (if this doesn't work, make sure you have the
  bash-completion package installed).

!!! tip

    It is possible to create custom completion files for your custom scripts

++alt+period++

Probably my second most used combination. It inserts **the last argument in the previous command**.

This sounds obscure, but is surprisingly good in many cases, because of how arguments in the CLI
is usually structured (e.g. filename goes last)

```bash
mkdir -p my_directory/with_the/longest-name_possible
cd [alt+.] # To navigate to the newly created directory

cat myfile
cp [alt+.] [alt+.].bak # Make a backup of the file

vim myscript.sh
./[alt+.] # To execute the script you just edited
git add [alt+.] # To stage it with version control

```

The list goes on.

++alt+0++

A variation of the previous hotkey. Will paste the first argument of the previous command (which is
actually the base command itself)

Useful when you have a difficult-to-write command like `firewall-cmd --add-port=123/udp` then 
`firewall-cmd --reload`, or something else where you want to repeat the command multiple times.

++ctrl+r++

Reverse command search. You know what your command is, but you don't feel like typing it all.
Just use ++ctrl+r++, then search for something unique to your command. Didn't find it on the first
try? Just run ++ctrl+r++ again for the next result. Mistyped? Use ++ctrl+c++ and try again.

`!!`

++ctrl+u++ and ++ctrl+y++

++ctrl+a++ and ++ctrl+e++

++ctrl+l++





## Navigation

`cd`

`cd -`

`cd ~user`
