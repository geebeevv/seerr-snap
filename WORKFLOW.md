# Automated Release Workflow Documentation

## Overview
This repository implements an automated system to track upstream Seerr releases and safely promote them through testing phases before reaching users. The workflow follows the pattern: **Upstream Release → Beta Testing → Stable Release**.

## Workflow Stages

### 1. Automatic Detection (Daily)
A GitHub Actions workflow executes daily at 2 AM UTC to check for new upstream releases by:
- Fetching the latest release from the seerr-team/seerr repository
- Comparing versions against the current snapcraft.yaml
- Initiating updates when new versions are detected

### 2. Beta Branch Update (Automatic)
When a new version is identified:
- Updates snapcraft.yaml on the beta branch with new version and source-tag
- Commits and pushes changes to beta
- Automatically creates a Pull Request from beta → main

### 3. Snapcraft Builds (Automatic)
When the beta branch updates:
- Snapcraft.io detects changes and builds the snap
- Publishes to the edge channel by default
- Users can test via: `snap install geebeevv-seerr --edge`

### 4. Pull Request Review (Manual)
The auto-generated PR includes upstream release notes, metadata, and testing checklists. Maintainers should:
- Review for breaking changes and security fixes
- Test the edge build
- Verify checklist items
- Optionally promote to beta channel manually
- Merge when satisfied

### 5. Stable Release (Semi-Automatic)
Upon PR merge:
- The main branch updates
- Snapcraft builds and publishes to edge
- Maintainers manually promote the build to stable channel via snapcraft.io

## Branch Strategy
- **main**: Stable releases (builds to edge, manually promoted to stable)
- **beta**: Testing releases (builds to edge, optional manual promotion to beta)

## Snap Channels

| Channel | Purpose | Install Command |
|---------|---------|-----------------|
| stable | Production (default) | `snap install geebeevv-seerr` |
| beta | Testing before stable | `snap install geebeevv-seerr --beta` |
| edge | Bleeding edge/daily | `snap install geebeevv-seerr --edge` |

## Manual Trigger
Access the Actions tab, select "Track Upstream Seerr Releases," and manually run the workflow on the main branch to check for updates immediately.

## Manual Channel Promotion

**To Beta**: Navigate to snapcraft.io → your snap → Releases tab, find the edge build from the beta branch, and release it to the beta channel.

**To Stable**: After merging the PR, go to snapcraft.io → Releases tab, find the edge build from main, and release it to stable.

Note: You can test directly from edge and skip the beta phase if preferred.

## Security Releases
For security-related upstream releases:
- Fast-track testing from edge
- Skip beta promotion
- Merge PR immediately
- Manually promote edge → stable right away

## Troubleshooting

**Workflow Not Running**: Check the Actions tab for errors; verify GitHub Actions is enabled and workflow files are on the main branch.

**PR Not Created**: Confirm no existing PR from beta → main exists (duplicates aren't created); review workflow logs.

**Snapcraft Not Building**: Verify snapcraft.io is connected to your GitHub repository and check the Builds tab for errors. Remember all branches build to edge by default.

**Version Mismatch**: Manually update snapcraft.yaml with correct version and source-tag, then commit to the appropriate branch.

## Configuration

### Change Schedule
Edit `.github/workflows/track-upstream.yml` to modify the cron schedule. Currently set to 2 AM UTC daily (`0 2 * * *`). Examples include:
- Every 6 hours: `0 */6 * * *`
- Weekly on Sunday: `0 0 * * 0`
- Monday and Thursday: `0 2 * * 1,4`

### Notifications
Enable notifications in repository Settings → Notifications for Pull Requests, or use GitHub's Watch feature.

## Best Practices
1. Always test beta builds before promoting
2. Review upstream release notes thoroughly
3. Fast-track security fixes immediately
4. Keep the beta branch clean—avoid manual changes

## FAQ

**Manual Updates**: Yes, you can edit snapcraft.yaml directly; the workflow continues tracking future releases.

**Skipping Versions**: Close the PR without merging; a new PR will be created for the next version.

**Local Testing**: Build locally using the `snapcraft` command before pushing.

---

**Workflow file**: `.github/workflows/track-upstream.yml`
**Maintainer**: geebeevv
**Upstream source**: https://github.com/seerr-team/seerr
