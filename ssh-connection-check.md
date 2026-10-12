# Checking SSH on Ubuntu

I wanted to understand how SSH works on my Ubuntu setup, so I checked its status, listening ports and recent logs.

## What I checked

- My current Linux user and hostname
- Whether the SSH socket was listening
- Which TCP ports were open for incoming connections
- Recent logs for the SSH socket

## Commands I used

```bash
whoami
hostname
systemctl status ssh.socket
ss -tln
journalctl -u ssh.socket -n 20 --no-pager
ssh prachi@localhost
```

## What I found

- My current user is `prachi` and my hostname is `lenovo`.
- `ssh.socket` is active and listening.
- Port 22 is listening on `0.0.0.0` and `[::]`, which cover IPv4 and IPv6 listening addresses.
- The logs show the socket being opened and closed at different times. It was listening when I checked its current status.

## What I learned

An inactive `ssh.service` does not always mean SSH is broken. When socket activation is configured, `ssh.socket` can listen for connections and trigger `ssh.service` when needed.

Checking the socket status, listening ports and logs helps me understand what is happening instead of relying on just one command.
## Connection test

I tested an SSH connection to my own machine using `ssh prachi@localhost`.

- The login succeeded.
- `whoami` confirmed that I was logged in as `prachi`.
- I used `exit` to close the SSH session normally.

## Why the connection test matters

A listening port shows that a service is accepting connections, but testing an actual login helps confirm that SSH access works end to end in this setup.
