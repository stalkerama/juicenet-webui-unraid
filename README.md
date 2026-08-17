# juicenet-webui-unraid

Docker/Unraid packaging for the upstream [WebForks/juicenet-webui](https://github.com/WebForks/juicenet-webui) project.

## Purpose

This repository exists only to make `juicenet-webui` easy to install and keep updated on Unraid.

**No changes are made to the original Juicenet WebUI application source.** The GitHub Actions workflow resolves the current upstream `main` commit and asks Docker Buildx to build the upstream repository's own Dockerfile directly at that exact commit.

Upstream project:

- https://github.com/WebForks/juicenet-webui

Container image:

- `ghcr.io/stalkerama/juicenet-webui-unraid:latest`

## Automatic upstream updates

The GitHub Actions workflow runs every 6 hours.

It:

1. Resolves the latest commit on `WebForks/juicenet-webui` `main`.
2. Checks whether an image tagged for that upstream commit already exists.
3. Skips the scheduled build when that exact upstream commit was already packaged.
4. Otherwise builds the upstream Dockerfile directly and publishes:
   - `ghcr.io/stalkerama/juicenet-webui-unraid:latest`
   - `ghcr.io/stalkerama/juicenet-webui-unraid:upstream-<commit>`

The workflow can also be run manually from **Actions -> Build Juicenet WebUI Unraid image -> Run workflow**.

## Unraid template

Template file:

- `unraid/juicenet-webui.xml`

Template URL:

```text
https://raw.githubusercontent.com/stalkerama/juicenet-webui-unraid/main/unraid/juicenet-webui.xml
```

The template exposes:

- WebUI port `3000`
- `/config` - persistent Juicenet/UI config and history
- `/data` - persistent Juicenet working data and generated NZBs
- `/jobs` - persistent queue jobs
- `/media` - user-selected source media/files, mounted read-only
- optional WebUI username/password/secret

Default persistent Unraid locations are under:

```text
/mnt/user/appdata/juicenet-webui/
```

The **Media** path is intentionally left blank so you can select the share/folder Juicenet should access.

## First install

After the first successful GitHub Actions build, make the GHCR package **Public** so Unraid can pull it without GitHub authentication.

Then install the XML template in Unraid and select the host path for **Media**.

Open the WebUI at:

```text
http://YOUR-UNRAID-IP:3000
```

Use the application's **Settings** page to configure Juicenet and place the required Nyuu configuration files in the mapped `/config` directory as required by the upstream project.

## Upstream application

All application functionality, dependencies, behavior, issues, and source code belong to the upstream project. This repository provides only packaging automation and the Unraid template.
