<!--documentation
---
title: Archipelago-deployment-live
tags:
  - Archipelago-deployment-live
---
documentation-->

# NYPR Commons

NYPR Commons is the digital repository of New York Public Radio's Media Library and Archives. It is a customization of the open-source software [Archipelago Commons](https://docs.archipelago.nyc/1.6.0/). Archipelago is maintained by [Metropolitan New York Library Council](https://metro.org/).

## Updates

### July 7, 2026

- Upgraded from Archipelago 1.6.0 to 2.1.0 and Drupal 10.6.8 to 11.3.13.

### June 1, 2026

- Updated README.md to include dev-ops recommendations for change management in our environments.

### May 30, 2026

- Simplified branches to have just `main` instead of `dev`, `staging`, and `production`.
- Changes are now managed in feature, issue, chore, and other branches that are merged into `main` via pull requests.

### May 28, 2026

-Incorporating NYPR webform customizations and other Drupal updates from Allison

### May 20, 2026

-Security update: Drupal core 10.6.3 to 10.6.8

### February 2026

- Upgraded to Drupal 10.6.3 and Archipelago 1.6.0 (from 1.5).
- Updated esmero-php to support audiowaveform.
- Upgraded strawberryfield to 1.7.0 to support local CSV-based linked open data endpoints.
- Added merge strategies to preserve certain environment-specific configuration files while continuing to track them in Git.

## Git Workflow for Regular Changes

If planning a more significant change that you'd like to do A-B user testing on, refer to internal admin documentation for a workflow that includes staging server evaluation before production.

### On dev EC2

From `/home/ec2-user/archipelago-deployment-live/`:

#### Step 1 — Check your branch, and make sure local main is up to date

```bash
git branch --show-current
git status
git diff
git pull --no-ff --no-edit
```

#### Step 2 — Create a feature branch BEFORE making any changes

```bash
git checkout -b chore/your-branch-name origin/main
git status
```

Confirm that you're on the new branch and have a clean working tree.

#### Step 3 — Make your changes

For Drupal configuration changes, make changes in the Drupal admin UI and then export:

```bash
docker-compose exec php bash -c "cd /var/www/html && drush config:export -y"
```

For file-only changes (README, scripts, etc.), edit files directly with your preferred editor.

#### Step 4 — Check what changed and discard environment-specific files

```bash
git diff --name-only
```

The checkouts below are necessary only when you've done a drush export.

```bash
git checkout -- drupal/config/sync/format_strawberryfield.iiif_settings.yml
git checkout -- drupal/config/sync/views.view.asset_children_as_table.yml
```

#### Step 5 — Stage ONLY your intentional changes

Never use `git add .` or `git add -A`.

```bash
git add drupal/config/sync/your-changed-file.yml
git commit -m "chore: description of change"
git push origin chore/your-branch-name
```

### On GitHub

#### Step 6 — Open a pull request

- Choose repository: `nypublicradio/archipelago-deployment-live`
- Select base branch: `main`
- Open a pull request from `chore/your-branch-name` → `main`
- Review the diff and verify only intended files are changed

#### Step 7 — Merge and clean up

- Squash and merge the pull request
- Delete the feature branch after merge

### On production EC2

From `/home/ec2-user/archipelago-deployment-live/`:

#### Step 8 — Preview and pull

```bash
git fetch origin
git diff HEAD origin/main
git pull
```

#### Step 9 — Apply config and clear caches

Skip this step for file-only changes such as README updates.

```bash
docker-compose exec php bash -c "cd /var/www/html && drush config:import -y && drush cache:rebuild"
```

### On dev and staging EC2s

#### Step 10 — On dev: return to main, sync, and clean up

```bash
git checkout main
git pull --no-ff --no-edit
git branch -D chore/your-branch-name
```

#### Step 11 — On staging: sync

```bash
git status
git diff
git checkout main
git pull --no-ff --no-edit
```

## Important Notes

- ALWAYS create the feature branch (Step 2) BEFORE making any changes.
- When on a feature branch, you have production site configuration in your branch — including files that are typically localized through our `merge = ours` strategy.
- Because of this, never start, stop, or restart Docker containers, or run `deploy.sh` or `update_deployed.sh` while working on a feature branch.
- Never push directly to `main` — always use feature branches and pull requests.
- Never use `git add .` or `git add -A` — always name specific files explicitly.
- Dev and staging must always use: `git pull --no-ff --no-edit`
- Production can use: `git pull`
- Staging shares S3 storage with production — never delete media or metadata records on staging.

## Source README

https://github.com/esmero/archipelago-deployment-live/blob/1.6.0/README.md

## License

[GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
