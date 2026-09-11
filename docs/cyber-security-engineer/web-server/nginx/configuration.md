# Nginx — Management and Configuration

Nginx's behavior is controlled through a configuration file (typically `nginx.conf`) made up of nested **contexts** — blocks that scope settings to a particular part of Nginx's behavior. Understanding which context a directive belongs in is the key to reading (and safely editing) an Nginx config.

## Configuration contexts

There are 5 types of context in an Nginx configuration:

| Context | Purpose |
|---|---|
| **Main** | Global settings, like worker processes. |
| **Events** | Handles connection processing. |
| **HTTP** | Configures HTTP server behavior. |
| **Server** | Defines settings for virtual hosts (server blocks). |
| **Location** | Specifies how to process requests for specific locations or URLs. |

## Key directives

### `user`

Defines which system user Nginx runs as:

```nginx
user www-data;
```

### `worker_processes`

Controls how many workers (CPU cores) Nginx uses to handle traffic:

```nginx
worker_processes auto;
```

### `worker_connection`

Controls how many concurrent connections each worker can handle:

```nginx
worker_connection 1024;
```

!!! tip
    The maximum number of clients Nginx can serve at once is `max_clients = worker_connection * worker_processes` — the two settings multiply together, so raising one without the other only gets you so far.

### The `server {}` block

HTTP-level settings need the `http` block configured first. Within it, a `server {}` block defines where the site's documents live and how requests are routed to them:

```nginx
server {
    listen 80;
    server_name example.com;
    root /var/www/html;
    index index.html
}
```

- `listen` — the port the server is accessed on.
- `server_name` — the name of the server (virtual host).
- `root` — where the site's files live on disk.

!!! note
    After pointing `root` at a document directory, the permissions on that directory need to be set correctly so the Nginx worker process (running as the `user` configured above) can actually read the files it's supposed to serve.
