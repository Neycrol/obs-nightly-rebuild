# Cloud-side OBS Nightly Rebuild

This setup drives a two-stage OBS nightly pipeline:

1. Refresh services and rebuild `home:neycrol:buildfactory-bootstrap` first (glibc producer).
2. Refresh services and rebuild `home:neycrol:buildfactory` second (single consumer repo).

The consumer project exposes a single pacman repository URL, so clients only need one custom repo.

## 1. Put workflow in a GitHub repository

Copy this file to your GitHub repo:

- `.github/workflows/obs-nightly-rebuild.yml`

## 2. Configure GitHub repository secrets

Required secrets:

- `OBS_USERNAME`: your OBS username
- `OBS_PASSWORD`: your OBS password (or app password/token if your instance supports it)

Optional secrets:

- `OBS_APIURL` (default: `https://api.opensuse.org`)
- `OBS_PROJECT` (default: `home:neycrol:buildfactory`)
- `OBS_REPO` (default: `buildfactory`)
- `OBS_BOOTSTRAP_PROJECT` (default: `home:neycrol:buildfactory-bootstrap`)
- `OBS_BOOTSTRAP_REPO` (default: `bootstrap`)
- `OBS_ARCH` (default: `x86_64`)

## 3. Schedule

Current schedule is once daily:

- `19:40 UTC`

Adjust cron lines in the workflow if needed.

## Single pacman repo

Use this published path:

- `https://downloadcontent.opensuse.org/repositories/home:/neycrol:/buildfactory/buildfactory/x86_64/`

Current db filename:

- `home_neycrol_buildfactory_buildfactory.db`

## Notes

- The workflow dynamically enumerates project packages and runs `runservice` only for packages that actually contain `_service`.
- Build order is enforced as: bootstrap -> main.
- Main project should contain `glibc-god` as an `_aggregate` package from bootstrap, so one repo remains sufficient for clients.
