# Dynamic forward using multiple Firefox containers

## Intro

So you want to use SSH dynamic forwarding with your browser.
Or maybe you need to use some internal SOCKS5 proxy to access your company services.

But you don't want to have a dedicated browser or profile for it, and you don't want to
deactivate the proxy settings every time you need something that the SOCKS5 proxy cannot
give you.

This post might solve your issues.

## More thorough intro

**In case you don't know what any of that means**, here is a quick overview of how you can use it:

Setting up Dynamic forward this way basically allows you to say *"Hey Firefox, please navigate to
this hostname and port, but do it as if you were actually running on the remote server I just SSH'ed
into."*

This is **very** useful when you want access to a Web UI for a server that you can normally only
reach via another server by logging in with SSH. It can easily be set up by specifying a port for
Dynamic Forwarding (for example `9999`), and editing your proxy settings either in the web browser
or in the system settings to point to (for example) `localhost:9999`.

Now you can enter a url like <https://my-service-web-ui:4321> in Firefox and it will navigate to it
via the internal server!

However, this server is (of course) securely installed inside a restricted network, and cannot reach
public URLs. So when you try to do some troubleshooting by navigating to <https://duckduckgo.com>,
your Firefox server might not be able to reach it, and your SSH terminal will get spammed with
`Connection refused`, `Access Denied`, `Forbidden` or similar, depending on your network setup.

Separating the different proxy configurations into different Multi-Account containers will allow you
to just open a tab in a container that represents *where your firefox should be connecting from*.

It would look something like this:

``` mermaid
graph LR
  A{Firefox}
  A -->|using| B(Default/None Container) -->|https:443| H>google.com]
  A -->|using| C(Server1 Container) -.->|ssh| X[Internal server1]
  X -->|https:443| I>internal service1]
  A -->|using| D(Server2 Container) -.->|ssh| Y[Internal server2]
  Y -->|https:8001| J>internal service2]

  C -->|SOCKS| X
  D -->|SOCKS| Y
```

**Side note**: Multi-account containers are also great to set up regardless of proxying/forwarding, for
example if you:

- have to log into multiple cloud consoles for AWX/Azure or others which use Single Sign-on, and
  you don't like having to log in and out or using Private browsing to achieve it.
- want to have something like a facebook, discord or youtube account logged in, but you don't want
  to mix work and personal profiles.

This setup has worked really well for me, you might want to give it a try!

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

Host another_destination
    Hostname some_other_other_hostname
    User firstlastname
    DynamicForward 8888 # <-- You can add as many as you want/need
    LogLevel error

```

1. Add this to supress "Connection refused" messages that appear every time
   you try to access a website that cannot be reached through the SOCKS proxy.

### Firefox part (Optional but recommended)

In Firefox, install the following extensions:

- Multi-Account Containers
- Sidebery (or any other plugin that lets to specify SOCKS5 proxy per container)

Add `localhost:9999` (or whichever port you specified in your SSH config) to Firefox,
or to the Multiaccount-container of your choice. You can create multiple containers too,
if you need access to services in multiple separated zones.


## Done

You can now browse whatever you would have access to on the remote server, as long as you:

1. have opened an SSH session to it
2. open it in the relevant container


