# Dynamic forward using multiple Firefox containers

## Intro

So you want to use SSH dynamic forwarding, or some other SOCKS5 proxy, but you
don't want to have a dedicated browser for it. This works well for me.

## Setup

### SSH part

Set up your SSH config with Dynamic forwarding, if necessary using jumphosts,
in your `~/.ssh/config`:

```shell hl_lines="11"

Host jumphost
    Hostname some_hostname
    User username
    Port 22
    IdentityFile ~/.ssh/id_ed25519_blablabla

Host final_destination
    Hostname some_other_hostname
    ProxyJump jumphost
    User username
    DynamicForward 9999 # <-- This is the important part
    LogLevel error # To supress connection refused messages

```

### Firefox part (Optional but reccommended)

In Firefox, install the following extensions:
- Multi-Account Containers
- Sidebery (or any other plugin that lets to specify SOCKS5 proxy per container)

Add `localhost:9999` (or whichever port you specified in your SSH config) to Firefox,
or to the Multiaccount-container of your choice


## Done

You can now browse whatever you would have access to on the remote server, as long as you:

#. have opened an SSH session to it
#. open it in the relevant container
