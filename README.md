# Seerr Snap

Unofficial snap package for [Seerr](https://github.com/seerr-team/seerr) - a media request and discovery manager for Jellyfin, Plex, and Emby.

## Description

Seerr is an automated media request and discovery manager that integrates with Jellyfin, Plex, and Emby media servers, as well as Sonarr and Radarr for content management.

**Official project**: https://github.com/seerr-team/seerr

## Known Issues

- Runs as a daemon with root privileges
- API-based communication only (no direct filesystem access to media libraries)
- Unofficial package - not supported by upstream Seerr developers

## Installation

### Stable Channel (Recommended)

```bash
sudo snap install geebeevv-seerr
```

The service autostarts on installation.

### Beta Channel (Early Testing)

Test new releases before they reach stable:

```bash
sudo snap install geebeevv-seerr --beta
```

Beta builds are automatically created when upstream releases new versions and go through testing before promotion to stable.

## Access

Web interface: http://localhost:5055

Configuration and database files are stored in `/var/snap/geebeevv-seerr/common/config/`

## Service Management

Control the service using standard snap commands:

```bash
sudo snap start geebeevv-seerr
sudo snap stop geebeevv-seerr
sudo snap restart geebeevv-seerr
```

View service logs:

```bash
sudo snap logs geebeevv-seerr
sudo snap logs geebeevv-seerr -f  # Follow mode
```

## Data Persistence

Data persists across snap updates in the common directory (`/var/snap/geebeevv-seerr/common/`).

## Maintenance

This repository uses automated GitHub Actions to track upstream releases. When Seerr releases a new version:

1. GitHub Actions automatically detects it
2. Updates the `beta` branch and triggers a Snapcraft build
3. Creates a Pull Request with release notes and testing checklist
4. After testing and approval, merging promotes to stable channel

See [WORKFLOW.md](WORKFLOW.md) for detailed documentation.

## License

See [LICENSE](LICENSE) file.
