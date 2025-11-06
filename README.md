# Seerr Snap

Unofficial snap package for [Seerr](https://github.com/seerr-team/seerr) - a media request and discovery manager for Jellyfin, Plex, and Emby.

## About

This is an **UNOFFICIAL** snap package of the Seerr application. Attempts were made to keep file size down while maintaining stability.

Seerr is a free and open source software application for managing requests for your media library. It integrates with Jellyfin, Plex, and Emby media servers, as well as Sonarr and Radarr.

Official project: https://github.com/seerr-team/seerr

## Installation

```bash
sudo snap install geebeevv-seerr
```

The service will autostart automatically on installation.

## Configuration

Configuration and database files are stored in `/var/snap/geebeevv-seerr/common/config/`

Access the web interface at: http://localhost:5055

## Manual Control

```bash
sudo snap start geebeevv-seerr
sudo snap stop geebeevv-seerr
sudo snap restart geebeevv-seerr
```

## Important Notes

- This snap runs as a service with root privileges
- The application communicates with Plex/Emby/Sonarr/Radarr via API calls
- Data persists across snap updates in the common directory

## License

See [LICENSE](LICENSE) file.
