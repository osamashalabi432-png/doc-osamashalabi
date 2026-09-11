# FortiWeb — Protected Servers

## Server health checks

FortiWeb continuously checks the health of the backend servers in a server pool, and can do so using several methods:

- TCP
- ICMP
- TCP Half Open
- TCP SSL
- HTTP

This lets FortiWeb stop sending traffic to a backend server that has gone down or is unresponsive, rather than forwarding requests to a dead server.

## Session persistence

Without persistence, each new client request might be forwarded to a *different* backend server in the pool — which can break application functionality that depends on server-local state:

- **Login sessions** — only the first server remembers the session, so a request that lands on a different server logs the user out.
- **Shopping carts** — items can "disappear" if the user's next request hits a different server that doesn't share session memory or cookies with the first.

Persistence keeps a given client's requests pinned to the same backend server for the duration of its session, avoiding these problems.
