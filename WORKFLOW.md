# Automated Release Workflow

This repository uses an automated workflow to track upstream Seerr releases and safely promote them through testing before releasing to users.

## Overview

```
Upstream Release → Beta Testing → Stable Release
     (auto)           (manual)        (auto)
```

## How It Works

### 1. Automatic Detection (Daily)

A GitHub Actions workflow runs daily at 2 AM UTC to check for new upstream releases:

- Fetches the latest release from [seerr-team/seerr](https://github.com/seerr-team/seerr)
- Compares with the current version in `snapcraft.yaml`
- If a new version is detected, proceeds to update

### 2. Beta Branch Update (Automatic)

When a new version is found:

1. Updates `snapcraft.yaml` on the `beta` branch:
   - Changes `version:` field to new version
   - Updates `source-tag:` to the new release tag
2. Commits and pushes to `beta` branch
3. Creates a Pull Request from `beta` → `main`

### 3. Snapcraft Builds (Automatic)

When the `beta` branch is updated:

- Snapcraft.io automatically detects the change and builds the snap
- The build is published to the **edge** channel by default
- Users can test with: `snap install geebeevv-seerr --edge`

### 4. Pull Request Review (Manual)

The auto-generated PR includes:

- **Upstream release notes** - What changed in the new version
- **Release metadata** - Version, publish date, pre-release flag
- **Testing checklist** - Items to verify before promoting
- **Direct link** to upstream release

**Your job:**

1. Review the upstream changes for:
   - Breaking changes
   - Security fixes (fast-track these!)
   - New dependencies or requirements
2. Test the edge build:
   ```bash
   snap refresh geebeevv-seerr --edge
   sudo snap logs geebeevv-seerr
   ```
3. Verify the checklist items in the PR
4. **Manually promote to beta channel** (if desired):
   - Go to snapcraft.io → your snap → **Releases** tab
   - Find the edge build
   - Click **"Release"** → select **beta** channel
   - Users can now test with: `snap refresh geebeevv-seerr --beta`
5. Merge the PR when satisfied

### 5. Stable Release (Semi-Automatic)

When you merge the PR:

- The `main` branch is updated
- Snapcraft automatically builds from `main` → publishes to **edge** channel
- **Manually promote to stable channel**:
  - Go to snapcraft.io → your snap → **Releases** tab
  - Find the latest edge build from `main` branch
  - Click **"Release"** → select **stable** channel
  - Users on stable get the update

## Branch Strategy

- **`main`** - Stable releases, builds to `edge` (manually promote to `stable`)
- **`beta`** - Testing releases, builds to `edge` (manually promote to `beta` if desired)

## Snap Channels

| Channel | Purpose | Install Command |
|---------|---------|----------------|
| `stable` | Production (default) | `snap install geebeevv-seerr` |
| `beta` | Testing before stable | `snap install geebeevv-seerr --beta` |
| `edge` | Bleeding edge/daily | `snap install geebeevv-seerr --edge` |

## Manual Trigger

You can manually trigger the upstream check without waiting for the daily schedule:

1. Go to **Actions** tab in GitHub
2. Select **Track Upstream Seerr Releases** workflow
3. Click **Run workflow**
4. Select `main` branch
5. Click **Run workflow**

## Manual Channel Promotion

Snapcraft.io automatically builds all branches to the **edge** channel. You need to manually promote builds to beta/stable:

### Promoting to Beta (Optional)
1. Go to [snapcraft.io](https://snapcraft.io) → your snap
2. Click the **Releases** tab
3. Find the edge build (from `beta` branch)
4. Click **"Release"**
5. Select **beta** channel
6. Confirm the release

### Promoting to Stable (After PR Merge)
1. Go to [snapcraft.io](https://snapcraft.io) → your snap
2. Click the **Releases** tab
3. Find the edge build (from `main` branch)
4. Click **"Release"**
5. Select **stable** channel
6. Confirm the release

**Note:** You can skip promoting to beta and test directly from edge if preferred.

## Security Releases

If an upstream release contains security fixes:

1. The workflow will still create a PR as normal
2. **Fast-track the process:**
   - Test quickly from edge
   - Skip beta promotion
   - Merge PR immediately
   - Manually promote edge → stable right away
3. The PR description will indicate if it's a pre-release

## Troubleshooting

### Workflow Not Running

- Check **Actions** tab for errors
- Verify GitHub Actions is enabled for the repository
- Ensure the workflow file is on the `main` branch

### PR Not Created

- Check if a PR from `beta` → `main` already exists
- The workflow won't create duplicates
- Check workflow logs in **Actions** tab

### Snapcraft Not Building

- Verify Snapcraft.io is connected to your GitHub repository
- Go to snapcraft.io → your snap → **Builds** tab
- Check for build errors or failed builds
- Note: All branches automatically build to `edge` channel by default
- You must manually promote to `beta` or `stable` via the **Releases** tab

### Version Mismatch

If the snap version gets out of sync:

1. Manually update `snap/snapcraft.yaml`:
   ```yaml
   version: '2.7.3'
   source-tag: v2.7.3
   ```
2. Commit to appropriate branch
3. The workflow will resume normal operation

## Configuration

### Change Schedule

Edit `.github/workflows/track-upstream.yml`:

```yaml
on:
  schedule:
    - cron: '0 2 * * *'  # Currently 2 AM UTC daily
```

Examples:
- `'0 */6 * * *'` - Every 6 hours
- `'0 0 * * 0'` - Weekly on Sunday
- `'0 2 * * 1,4'` - Monday and Thursday at 2 AM

### Notifications

To get notified of new PRs:

1. Go to repository **Settings** → **Notifications**
2. Enable notifications for Pull Requests
3. Or use GitHub's "Watch" feature

### Customize PR Labels

In the workflow file, change the label:

```yaml
--label "upstream-update" \
```

## Best Practices

1. **Don't skip testing** - Always test beta builds before promoting
2. **Read release notes** - Check for breaking changes
3. **Monitor beta users** - If you have beta testers, get their feedback
4. **Fast-track security** - Merge security fixes immediately
5. **Keep branches clean** - Don't make manual changes to `beta` branch

## FAQ

**Q: Can I manually update the version?**
A: Yes! Just edit `snapcraft.yaml` on the appropriate branch and commit. The workflow will continue to track future releases.

**Q: What if I want to skip a version?**
A: Close the PR without merging. The workflow will create a new PR for the next version.

**Q: Can I test locally before beta?**
A: Yes! Build locally with `snapcraft` command before pushing.

**Q: What if upstream releases too frequently?**
A: PRs won't stack - only one PR at a time. Review and merge each one, or adjust the schedule.

---

**Workflow file:** `.github/workflows/track-upstream.yml`
**Maintainer:** geebeevv
**Upstream:** https://github.com/seerr-team/seerr
