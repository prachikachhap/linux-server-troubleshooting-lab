# Investigating an SSH Connection Refused Error

## Problem

I tried connecting to my Ubuntu machine using SSH on port 2222 and received a `Connection refused` error.

## Command used

```bash
ssh -p 2222 prachi@localhost
```

## Error observed

```text
ssh: connect to host localhost port 2222: Connection refused
```

## Investigation

I checked the listening TCP ports using:

```bash
ss -tln
```

The output showed port 22 listening on IPv4 and IPv6 addresses. No listener was shown on port 2222.

## Finding

The SSH connection failed because no service was accepting the connection on port 2222 at the time of the test. SSH was listening on port 22.

## What I learned

The `-p` option specifies the port used by the SSH client. A connection can fail when the requested port has no listening service.

Checking the exact error message and listening ports helps narrow down the cause instead of changing settings without evidence.

## Test environment

Ubuntu running in WSL2. The test was performed against `localhost`, not a remote production server.
