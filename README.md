# nginx pages — static site & error pages

Our custom nginx pages that share the same layout, typography, and background treatment.

## Contents

| File | Purpose |
|------|---------|
| `index.html` | Default nginx page |
| `error_400.html` | 400 Bad Request |
| `error_403.html` | 403 Forbidden |
| `error_404.html` | 404 Not Found |
| `error_405.html` | 405 Method Not Allowed |
| `error_413.html` | 413 Payload Too Large |
| `error_429.html` | 429 Too Many Requests |
| `error_500.html` | 500 Internal Server Error |
| `error_502.html` | 502 Bad Gateway |
| `error_503.html` | 503 Service Unavailable |
| `error_504.html` | 504 Gateway Timeout |

## Deploy

1. Copy the HTML files to your nginx document root (example: `/var/www/html`).

2. Configure nginx to use the custom error pages:

```nginx
server {
    listen 80 default_server;
    server_name _;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    error_page 400 /error_400.html;
    error_page 403 /error_403.html;
    error_page 404 /error_404.html;
    error_page 405 /error_405.html;
    error_page 413 /error_413.html;
    error_page 429 /error_429.html;
    error_page 500 /error_500.html;
    error_page 502 /error_502.html;
    error_page 503 /error_503.html;
    error_page 504 /error_504.html;

    location = /error_400.html { internal; }
    location = /error_403.html { internal; }
    location = /error_404.html { internal; }
    location = /error_405.html { internal; }
    location = /error_413.html { internal; }
    location = /error_429.html { internal; }
    location = /error_500.html { internal; }
    location = /error_502.html { internal; }
    location = /error_503.html { internal; }
    location = /error_504.html { internal; }
}
```

3. Test and reload nginx:

```bash
sudo nginx -t && sudo systemctl reload nginx
```

## Nginx behavior

- `error_page` maps each status code to the matching `/error_*.html` file.
- `location = /error_*.html { internal; }` prevents direct access to error pages.
- `try_files $uri $uri/ =404;` ensures missing files trigger the custom 404 page properly.

## Customizing

Each error page is self-contained (embedded styling, fonts, layout). To change branding or text, edit the relevant HTML file sections like `<title>`, `.code`, `.label`, and `.desc`.

To add new error pages, duplicate an existing file and add corresponding `error_page` and `location` directives in nginx.


## License

MIT License — see [LICENSE](LICENSE) for details.
