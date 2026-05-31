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
If planning a more significant change that you'd like to do A-B user testing on etc, refer to internal admin documentation for a workflow that includes the changes being rolled out on staging server and evaluated before being pushed to the production server.

### On dev EC2
From `/home/ec2-user/archipelago-deployment-live/`:

    # 1. Make sure local main is up to date
    git pull --no-ff --no-edit

    # 2. Create a feature branch
    git checkout -b chore/your-branch-name origin/main

    # 3. Make your changes
    nano README.md  # or whatever files you're editing

    # 4. Commit and push the feature branch
    git add <changed-files>
    git commit -m "chore: description of change"
    git push origin chore/your-branch-name


### On GitHub
    5. Open PR: chore/your-branch-name → main
    6. Review and merge PR
    7. Delete the feature branch (button after merge)

### On production EC2
From `/home/ec2-user/archipelago-deployment-live/`:

    # 8. Pull to production
    git pull

### On dev and staging EC2s
    # 9. On dev and staging — sync local main when convenient
    git pull --no-ff --no-edit

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
