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

## Source README
https://github.com/esmero/archipelago-deployment-live/blob/1.6.0/README.md

## License

[GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
