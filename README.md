# Dockerized Media Server

## Features

- Automatically download movies from watchlist in trakt.tv
- Automatically download subtitles in spanish and english (configurable) from [opensubtitles.org](https://opensubtitles.org) or [podnapisi.net](https://podnapisi.net)
- Plex Media Server

---

## Prerequisites

Software:

- docker
- docker-compose

Configure [secrets.yml](/config/secrets.yml) with valid credentials:

- *transmission*: User and pass matching those in [docker-compose.yml](/docker-compose.yml)
- *opensubtitles*: Credentials from your opensubtitles.org account

## Configuration

Besides service credentials, [secrets.yml](/config/secrets.yml) also contains configuration variables for trakt watchlist and subtitle languages:

- *trakt*:
  - *user*: User who owns the watchlist
  - *list*: Name of the watchlist
- *subtitles*:
  - *lang*: Language for subtitles
  - *alt_lang*: Alternative language for subtitles

## Execution

```bash
docker-compose up
```

- Plex Media Server: http://localhost:32400
  - If you login with your Plex account you'll have access to your library from everywhere.

To see/debug the magic behind the scenes:
- Tranmission Web UI: http://localhost:9091
- Flexget Web UI: http://localhost:5050