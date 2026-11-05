# MeshCentral PR test images

Unofficial Docker images for testing my open pull requests on
[Ylianst/MeshCentral](https://github.com/Ylianst/MeshCentral) without building anything locally.

Each image is the **latest upstream `master` with the PR merged on top**: no other PRs.
Built for `linux/amd64` and `linux/arm64` with the upstream Dockerfile (full Alpine variant).

| Tag | Contents |
|---|---|
| `ghcr.io/samschultzponsys/meshcentral:pr-<number>` | upstream master + that one PR |
| `ghcr.io/samschultzponsys/meshcentral:my-prs` | upstream master + all my open PRs that merge together |
| `…:pr-<number>-<YYYYMMDD-HHMM>` | a specific build that never changes |

```bash
docker pull ghcr.io/samschultzponsys/meshcentral:pr-8299
```

or in `compose.yaml`, replace the image line with `image: ghcr.io/samschultzponsys/meshcentral:pr-8299`.

**Please test on a copy, not production.** Back up your `meshcentral-data` folder first; these are
not official releases.

Images rebuild automatically (checked hourly) when upstream master or the PR gets new commits.
To build right away: Actions → *PR test images* → *Run workflow*.
See [the workflow](.github/workflows/pr-test-images.yml) for exactly how they are built.
