# dockerbase-wordpress

> A zero-config Docker environment for WordPress theme and plugin development — works on any OS.

[![Docker](https://img.shields.io/badge/Docker-ready-2496ED?logo=docker)](https://www.docker.com/)
[![WordPress](https://img.shields.io/badge/WordPress-dev-21759B?logo=wordpress)](https://wordpress.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## Overview

`dockerbase-wordpress` spins up a fully isolated WordPress stack with a single command.  
No XAMPP, no MAMP, no manual PHP/MySQL installs — just Docker.  
The `content/` directory maps directly into `wp-content`, so your themes and plugins live in version control and hot-reload in the browser immediately.

## Stack

| Service    | Notes                              |
|------------|------------------------------------|
| WordPress  | php8.3-fpm-alpine  , latest stable |
| Database   | MySQL 9                            |
| Web server | Nginx alpine                       |

*(Exact versions are pinned in `docker/` and `docker-compose.yml`.)*

---

## Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (macOS / Windows)  
  or Docker Engine + Compose plugin (Linux)
- Git

---

## Quick Start

```bash
git clone https://github.com/navidnasro/dockerbase-wordpress.git
cd dockerbase-wordpress
docker compose up -d
```

Open `http://localhost:8080` and complete the WordPress 5-minute install.

> **Default credentials** are set in `docker-compose.yml` under the `db` service —  
> change them before any public or shared exposure.

---

## Project Structure

```
dockerbase-wordpress/
├── content/            # Mounted as wp-content — put themes & plugins here
├── docker/             # Dockerfiles, Nginx config, PHP-FPM config
├── docker-compose.yml  # Service orchestration (WordPress, MySQL, Nginx)
└── .gitignore
```

---

## Developing Themes & Plugins

1. Place your theme in `content/themes/<your-theme>/`
2. Place your plugin in `content/plugins/<your-plugin>/`
3. Activate from the WordPress admin at `http://localhost:8080/wp-admin`

File changes are reflected immediately — no container restart needed.

---

## Useful Commands

| Task                       | Command                                       |
|----------------------------|-----------------------------------------------|
| Start containers           | `docker compose up -d`                        |
| Stop containers            | `docker compose down`                         |
| Stop + wipe database       | `docker compose down -v`                      |
| Tail logs                  | `docker compose logs -f`                      |
| WordPress shell            | `docker compose exec wordpress bash`          |
| WP-CLI                     | `docker compose exec wordpress wp <command>`  |

---

## Configuration

Environment variables are in `docker-compose.yml`:

```yaml
WORDPRESS_DB_HOST:     db
WORDPRESS_DB_NAME:     wordpress
WORDPRESS_DB_USER:     wordpress
WORDPRESS_DB_PASSWORD: secret
WORDPRESS_DEBUG:       "1"   # set to 0 for production-like testing
```

Nginx and PHP-FPM settings live in `docker/`. Edit them and run `docker compose restart` to apply.

---

## Notes

- **Persistence** — MySQL data lives in a named Docker volume. `docker compose down -v` deletes it permanently.
- **Port conflicts** — If `8080` is in use, change the port mapping in `docker-compose.yml` (e.g., `"3000:80"`).
- **Windows line endings** — Ensure shell scripts inside `docker/` are saved with LF, not CRLF.

---

## Contributing

Pull requests are welcome. For significant changes, open an issue first to discuss the approach.

## License

[MIT](LICENSE)
