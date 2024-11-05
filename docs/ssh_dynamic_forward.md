# Dynamic forward using multiple Firefox containers

## Intro

So you want to use SSH dynamic forwarding with your browser.
Or maybe you need to use some internal SOCKS5 proxy to access your company services.

``` mermaid
graph LR
  A{Firefox} -->|via| B(Container1) -->|https| H>google.com]
  A{Firefox} -->|via| C(Container2) -->|ssh| X[internal server1] -->|https| I>internal service1]
  A{Firefox} -->|via| D(Container3) -->|ssh| Y[internal server2] -->|https| J>internal service2]
```

But you don't want to have a dedicated browser or profile for it, and you don't want to
deactivate the proxy settings every time you need something that the SOCKS5 proxy cannot
give you.

The following setup works really well in my situation:

## Setup

### SSH part

Set up your SSH config with Dynamic forwarding, if necessary using jumphosts,
in your `~/.ssh/config`:

```shell hl_lines="11 12"

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
    LogLevel error # (1)!

```

1. Add this to supress "Connection refused" messages that appear every time
   you try to access a website that cannot be reached through the SOCKS proxy.

### Firefox part (Optional but recommended)

In Firefox, install the following extensions:

- Multi-Account Containers
- Sidebery (or any other plugin that lets to specify SOCKS5 proxy per container)

Add `localhost:9999` (or whichever port you specified in your SSH config) to Firefox,
or to the Multiaccount-container of your choice


## Done

You can now browse whatever you would have access to on the remote server, as long as you:

1. have opened an SSH session to it
2. open it in the relevant container


