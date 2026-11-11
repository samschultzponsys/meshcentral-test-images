# MeshCentral PR test images

Unofficial Docker images for testing my open pull requests on
[Ylianst/MeshCentral](https://github.com/Ylianst/MeshCentral) without building anything locally.

Each image is the **latest upstream `master` with the PR merged on top**: no other PRs.
Built for `linux/amd64` and `linux/arm64` with the upstream Dockerfile (full Alpine variant).

| Tag | Contents |
|---|---|
| `ghcr.io/samschultzponsys/meshcentral:pr-<number>` | upstream master + that one PR |
| `ghcr.io/samschultzponsys/meshcentral:my-prs` | upstream master + all my open PRs combined |
| `…:pr-<number>-<YYYYMMDD-HHMM>` | a specific build that never changes |

```bash
docker pull ghcr.io/samschultzponsys/meshcentral:pr-8299
```

or in `compose.yaml`, replace the image line with `image: ghcr.io/samschultzponsys/meshcentral:pr-8299`.

**Please test on a copy, not production.** Back up your `meshcentral-data` folder first; these are
not official releases.

Images rebuild automatically (checked hourly) when upstream master or the PR gets new commits.
Fixed build tags are kept for 30 days; all tags of merged/closed PRs are removed (cleanup runs daily).
To build right away: Actions → *PR test images* → *Run workflow*.
See [the workflow](.github/workflows/pr-test-images.yml) for exactly how they are built.

## When PRs overlap (`my-prs`)

Some PRs change the same lines as each other. Upstream sees each PR on its own, so that only matters
once one of them is merged, but `my-prs` stacks them all. The places where they overlap were resolved
by hand once (keeping both changes) and saved in [`rerere/`](rerere); git re-applies a saved
resolution automatically whenever it meets exactly the same overlap again. A resolution is only used
if no conflict markers are left and every changed `.js`/`.json` file still parses.

If a PR later changes the overlapping lines, its saved resolution no longer fits: that PR is then left
out of `my-prs` with a warning in the run summary (its own `pr-<number>` image is unaffected) until the
overlap is resolved again and the new files in `.git/rr-cache` are copied into `rerere/`.
