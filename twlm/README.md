# TWLM build of SigNoz

Branch `twlm/v0.142.1` = upstream tag `v0.142.1` + a two-line frontend patch
that shows **Integrations** on community self-hosted (the backend already
serves the integrations API without a licence):

- `frontend/src/container/SideNav/SideNav.tsx` — sidebar item no longer
  requires Cloud / Enterprise (admin or editor role still required).
- `frontend/src/AppRoutes/index.tsx` — the `/integrations` route is kept.

`twlm/Dockerfile` rebuilds only the frontend and layers it onto the official
`signoz/signoz:v0.142.1` image, so the backend binary is the official one.
`.github/workflows/twlm-image.yaml` publishes it to
`ghcr.io/twlm-app/signoz:v0.142.1-twlm.N`.

## Upgrading SigNoz

1. `git fetch upstream tag vX.Y.Z` and branch `twlm/vX.Y.Z` from it.
2. Cherry-pick the patch commit; resolve if those two files changed.
3. Bump `SIGNOZ_BASE` / `VITE_VERSION` in `twlm/Dockerfile` and the tag in
   the workflow; push the branch.
4. Point `SIGNOZ_IMAGE` in ops-observability at the new tag.
