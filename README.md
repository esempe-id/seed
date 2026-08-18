# seed

Docker images for the Esempe panel runners.

This repository contains scripts to build patched Temurin images for runners, with additional system
packages needed by the Esempe panel runners.

## Repository structure

```
temurin/   Temurin-based images (one directory per Java version)
base/      Base OS images (for example, Ubuntu or Debian)
scripts/   Helper scripts 
```

## Temurin images

Every version directory under `temurin/` (e.g. `temurin/25`) holds:

- `package.extras` extra APT packages to install, one per line
  (plain text; blank lines and lines starting with `#` are ignored)
- `Dockerfile` **auto-generated**, do not edit by hand

The generated Dockerfile is based on `eclipse-temurin:<version>-jre-resolute` (Ubuntu 26.04 / Resolute)
and always installs the standard runner packages:

- `curl`
- `jq`
- `unzip`
- `wget`
- `bash`
- `ca-certificates`

plus the extras listed in `package.extras`. The image is tagged as:

```
ghcr.io/esempe-id/eclipse-temurin:<version>-jre
```

### Usage

```bash
# Regenerate all Dockerfiles
python3 scripts/generate_dockerfiles.py

# Regenerate a single version
python3 scripts/generate_dockerfiles.py --version 21

# Create a package.extras template for a new version directory
python3 scripts/generate_dockerfiles.py --version 30 --init-extras

# Print the resulting image tag(s), e.g. for CI tagging
python3 scripts/generate_dockerfiles.py --print-tags

# CI drift check (exit code 1 when Dockerfiles are out of date)
python3 scripts/generate_dockerfiles.py --check
```

After editing `package.extras`, rerun the generator so the Dockerfile reflects the new package list.
