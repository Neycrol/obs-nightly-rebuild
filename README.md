# Cloud-side OBS Nightly Rebuild

This setup triggers a full rebuild of the OBS project from GitHub Actions on a schedule.
No local computer is required.

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
- `OBS_ARCH` (default: `x86_64`)

## 3. Schedule

Current schedule is twice daily:

- `19:40 UTC` (Europe evening)
- `03:40 UTC` (US evening)

Adjust cron lines in the workflow if needed.

## Notes

- This triggers rebuilds only; it doesn't alter package sources.
- OBS source services (`tar_scm`) run server-side during rebuild chain as needed.
- Rebuild overlap is naturally handled by OBS scheduler, but you can reduce frequency if queue pressure is high.
