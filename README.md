# Seerr Snap

Unofficial snap package for [Seerr](https://github.com/seerr-team/seerr) - a media request and discovery manager for Jellyfin, Plex, and Emby.

## About

This is an **UNOFFICIAL** snap package of the Seerr application. Attempts were made to keep file size down while maintaining stability.

Seerr is a free and open source software application for managing requests for your media library. It integrates with Jellyfin, Plex, and Emby media servers, as well as Sonarr and Radarr.

Official project: https://github.com/seerr-team/seerr

## Installation

### Stable Channel (Recommended)

```bash
sudo snap install geebeevv-seerr
```

The service will autostart automatically on installation.

### Beta Channel (Early Testing)

Help test new releases before they reach stable:

```bash
sudo snap install geebeevv-seerr --beta
```

Beta builds are automatically created when upstream releases new versions and go through testing before promotion to stable.

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

## For Maintainers

This repository uses an automated workflow to track upstream releases. When Seerr releases a new version:

1. GitHub Actions automatically detects it
2. Updates the `beta` branch and triggers a Snapcraft build
3. Creates a Pull Request with release notes and testing checklist
4. After testing and approval, merging promotes to stable channel

See [WORKFLOW.md](WORKFLOW.md) for detailed documentation.

## License

See [LICENSE](LICENSE) file.
