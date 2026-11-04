# pod-crabbox-browser

The **browser lease image** for the Crabbox `containerd` provider — the base
[`pod-crabbox`](https://github.com/opencharly/pod-crabbox) lease contract with Chromium,
Firefox and a `crabbox-browser` launcher that keeps the Chromium sandbox enabled.

## What it provides

| Property | Value |
|---|---|
| Base | CachyOS base image `ghcr.io/opencharly/pod-crabbox:2026.307.1828` |
| Browsers | Chromium (Arch repo package) and Firefox |
| Launcher | `/usr/local/bin/crabbox-browser` |
| Sandbox | ON — never `--no-sandbox`, never `seccomp=unconfined` |

## How to use it

```sh
ssh crabbox@<lease> crabbox-browser https://example.com
# on crun/runc the lease must run with the host's narrow seccomp profile:
#   --security-opt seccomp=/etc/containerd/seccomp/crabbox-userns.json
# under krun the guest kernel is the boundary
```

## Layout

- `charly.yml` — the `pod-crabbox-browser` image (pinned base image + entrypoint).
- `candy/` — the variant's own layer: packages, files and `check:` steps.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `.github/workflows/image.yml` — builds on `pull_request` and publishes at the merge
  tag, so the image tag equals the git tag.
- `CHANGELOG/` — per-CalVer history.

## Related

- [`opencharly/pod-crabbox`](https://github.com/opencharly/pod-crabbox) — the lease
  base image this family builds on.
- [`CONTAINERD-IMAGES.md`](https://github.com/opencharly/opencharly/blob/main/CONTAINERD-IMAGES.md)
  §2 — the lease-image contract.
