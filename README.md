<!--documentation
---
title: Archipelago-deployment-live
tags:
  - Archipelago-deployment-live
---
documentation-->

# NYPR Commons

NYPR Commons is the digital repository of New York Public Radio's Media Library and Archives. It is a customization of open-source software, [Archipelago Commons](https://docs.archipelago.nyc/1.6.0/) Archipelago is maintained by [Metropolitan New York Library Council](https://metro.org/).  

## Updates:

May 30, 2026: 
Simplified branches to have just MAIN instead of dev, staging, production. Will manage changes in feature, issue, chore, etc branches which will be PRd into main.

May 2026:
Updated this README.md, testing new change management pipeline.

February 2026:
February 2026 Upgraded to Drupal 10.6.3 and Archipelago 1.6.0 (from 1.5) and update esmero-php to allow for audiowaveform, and upgrade strawberryfield to 1.7.0 to allow for local CSV-based linked open data endpoints. Includes merge strategy to prefer existing file for certain environment specific configs that we still want to track in git.


## Git Workflow for Regular Changes

### On dev EC2 (ip-172-31-12-134)
From `/home/ec2-user/archipelago-deployment-live/`:

    # 1. Make sure local main is up to date
    git pull --no-ff --no-edit

    # 2. Create a feature branch
    git checkout -b chore/your-branch-name

    # 3. Make your changes
    nano README.md  # or whatever files you're editing

    # 4. Commit and push the feature branch
    git add <changed-files>
    git commit -m "chore: description of change"
    git push origin chore/your-branch-name

### On staging EC2 (ip-172-31-4-159) — test BEFORE merging to main
From `/home/ec2-user/archipelago-deployment-live/`:

    # 5. Check out the feature branch on staging
    git fetch origin
    git checkout -b chore/your-branch-name origin/chore/your-branch-name

    # 6. Test on staging (archipelago-staging.nyprarchives.org)
    #    Staging has S3 and a full collection — validate in a production-like env.
    #    For non-trivial changes: have at least one user test before promoting.
    #
    #    ARNING: Staging shares S3 storage with production.
    #    Do NOT delete media files or their associated metadata records while
    #    testing on staging — deletions will permanently remove files from
    #    production storage.

    # 7. When staging is confirmed good, return to main
    git checkout main

### On GitHub — only merge after staging confirms good
    8. Open PR: chore/your-branch-name → main
    9. Review and merge PR
    10. Delete the feature branch (button after merge)

### On staging EC2 — sync with merged main
    # 11. Pull the merged change
    git pull --no-ff --no-edit

### On production EC2 (ip-172-31-34-119)
From `/home/ec2-user/archipelago-deployment-live/`:

    # 12. Pull to production
    git pull

### Important Notes
- Never push directly to main from dev or staging — always use feature branches + PRs
- Dev and staging must always use: git pull --no-ff --no-edit
- Production can use plain git pull
- Staging is the gate for production — don't skip it for non-trivial changes
- Staging shares S3 storage with production — never delete media or metadata records on staging


## Source README
https://github.com/esmero/archipelago-deployment-live/blob/1.6.0/README.md

## License

[GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
