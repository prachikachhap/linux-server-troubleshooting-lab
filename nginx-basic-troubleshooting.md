# NGINX Basic Troubleshooting

## Objective

Practise checking an NGINX web server, testing HTTP responses, reading logs, and locating its configuration and website files.

## 1. Check the service and port

Commands used:

```bash
sudo systemctl status nginx
ss -tln
```

Observations:
- NGINX was active and running.
- Port 80 was listening for HTTP connections.

## 2. Test the HTTP response

Command:

```bash
curl -i http://localhost
```

Observation:
- NGINX returned `HTTP/1.1 200 OK`.
- The default NGINX welcome page was returned.

## 3. Inspect the logs

Commands:

```bash
sudo tail -n 10 /var/log/nginx/access.log
sudo tail -n 10 /var/log/nginx/error.log
```

Observations:
- The access log recorded successful homepage requests with status `200`.
- A browser request for `/favicon.ico` returned `404`.
- A request for `/abc` also returned `404`.
- The displayed error log contained a startup notice, not an error.

## 4. Inspect the configuration

Main configuration:

```text
/etc/nginx/nginx.conf
```

Default website configuration:

```text
/etc/nginx/sites-available/default
```

Enabled configuration:

```text
/etc/nginx/sites-enabled/default
```

The `default` entry in `sites-enabled` is a symbolic link to the configuration in `sites-available`.

Important settings in the default server configuration:

```nginx
listen 80 default_server;
root /var/www/html;
index index.html index.htm index.nginx-debian.html;
server_name _;
location / {
    try_files $uri $uri/ =404;
}
```

## 5. Investigate the 404 responses

Command:

```bash
ls -l /var/www/html
```

Observation:
- The directory contained `index.nginx-debian.html`.

Command:

```bash
ls -l /var/www/html/favicon.ico
```

Observation:
- The favicon file did not exist at that path.
- This explains why the request for `/favicon.ico` returned `404` under the current configuration.

The `/abc` request also returned `404`, indicating that NGINX could not find a matching resource for that URL.

## Key learnings

- A running service does not guarantee that every URL works.
- Access logs help identify requested paths and HTTP status codes.
- The NGINX configuration determines where website files are served from.
- File permissions must allow NGINX to read the files it serves.
- A `404` response is different from a server-side `500` error.

## Next step

Test the NGINX configuration with `sudo nginx -t` before learning how to reload configuration changes safely.
