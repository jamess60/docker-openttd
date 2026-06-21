# CHANGELOG.md


## v5 (2026-06-21)
Features:
  - Update openttd to 15.3
  - General container base image update


## v4 (2025-06-16)
Features:
  - Update openttd to 14.1
  - General container base image update
  - Add healthcheck to image
  - Drop support for linux/s390x (Whose using that anyway...)


## v3 (2025-04-23)
Features:
  - Add curl & wget to enable docker healthcheck support

Fix:
  - Remove apt upgrade to make image smaller, debian:latest should be suffiscient. 



## v2 (2025-04-22)
Features:
  - Update container base image


## v1 (2025-01-01)
Features:
  - Initial commit based on OpenTTD 14.1 (latest stable) and Debian 12

Fix:
  - N/A

