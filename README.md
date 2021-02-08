# Dockerized Media Server

Media Server originally built for execution on Raspberry Pi

## Features

- Automatically download movies from watchlist in trakt.tv.
- Automatically download subtitles in spanish and english (configurable) from [opensubtitles.org](https://opensubtitles.org) or [podnapisi.net](https://podnapisi.net).
- Plex Media Server.

---

## Prerequisites

Software:

- docker
- docker-compose

Configure [.env](/.env):

- *MOVIES*: Directory for downloaded movies.
- *STORAGE*: Directory for Plex Library, temporary files, databases, configuration...
- *FLEXGET_PASS*: Password for Flexget Web UI.
- *TRANSMISSION_USER*: User for transmission.
- *TRANSMISSION_PASS*: Password for transmission.
- *PLEX_CLAIM*: Claim code to automatically claim Plex Media Server. You can get a temporal claim code at [plex.tv/claim](https://www.plex.tv/claim/).

Configure [secrets.yml](/flexget/config/secrets.yml) with valid credentials:

- *transmission*: User and pass matching those in [.env](/.env).
- *opensubtitles*: Credentials from your opensubtitles.org account.

## Configuration

Besides service credentials, [secrets.yml](/flexget/config/secrets.yml) also contains configuration variables for trakt watchlist and subtitle languages:

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

To see/debug the magic behind the scenes:
- Tranmission Web UI: http://localhost:9091
- Flexget Web UI: http://localhost:5050