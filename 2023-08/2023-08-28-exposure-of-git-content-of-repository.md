# Exposure of .git content via publicly accessible web resources.

## Issue Summary

Due to a missing configuration setting `NODE_ENV=production`, the 
non-public files in the git repository of Chia’s Discord bot were copied over and made publicly available.

A private GITHUB_TOKEN was exposed publicly, that could have allowed the private repository of the Chia Discord bot to be cloned without access rights. The GITHUB_TOKEN lifespan was very limited.


## Timeline (all times PST) 08/23/2023 - 08/23/2023. All times approximations

08/23/2023 - A researcher submitted a bug report via bugcrowd (https://bugcrowd.com/chia-network-bb) detailing the security issue.

08/23/2023 - The .git folder was removed from the image that was serving the public resources. 

08/23/2023 - The configuration setting `NODE_ENV=production` was added.


## Resolution and recovery

This issue was addressed by correcting the configuration setting that caused the non-public files to be copied as part of the image, and removing the non-public files (.git folder) from the image.
