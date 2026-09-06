---
name: exposing-services
description: Makes a service on this laptop reachable from the company LAN, or finds out why one is not answering. Use when binding a dev server or container to anything other than localhost, when a service must be reachable from the network, or when a service does not answer from another machine.
---

# Exposing services

ufw is on: incoming DROP, outgoing ACCEPT, routed DROP. Interface `wlo1`, company LAN `10.0.30.0/24`. Without an explicit rule nothing from the LAN reaches this machine. The user's explicit instructions take precedence over this skill.

## Defaults

- Bind dev servers to `127.0.0.1`.
- Docker publishes on `127.0.0.1` by default (`ip` in `/etc/docker/daemon.json`), so `-p 8080:80` stays local. The `DOCKER-USER` chain drops new inbound connections from `wlo1` and `eno2`, so a container stays local even when published on `0.0.0.0`.

## Exposing a port to the LAN

1. Ask the user, naming the service, the port and the protocol.
2. `sudo fw-temp open <port> [tcp|udp]`. The rule covers only `wlo1` and `10.0.30.0/24`. `sudo fw-temp list` shows what is open.
3. `fw-temp open` opens the firewall and changes no bind. A container additionally needs `-p 0.0.0.0:<port>:<port>`; a dev server needs its own host option set to `0.0.0.0`.
4. When the task is finished: `sudo fw-temp close`.

## A service does not answer from the network

`sudo ufw status verbose`, then `journalctl -k -g 'UFW BLOCK' --since -15min`. ufw rate-limits its log lines, so an empty log proves nothing. `/var/lib/fw-listen/wildcard-listeners.txt` lists the processes listening on a wildcard address, refreshed every five minutes; changes to that list go to `journalctl -t fw-listen`. Check in this order: the process listens on `0.0.0.0` or the LAN address rather than `127.0.0.1` only; the port appears in `sudo fw-temp list`; a container publishes on `0.0.0.0`.

## Limits

`ufw disable`, `ufw reset` and iptables flushes are denied by permissions; when one seems needed, raise it with the user together with the reason. A root command the user must run themselves is written as `pkexec ...` so a password dialog appears.

## Done when

A task that opened a port ends with `sudo fw-temp close`, or with an explicit statement that the port stays open and why.
