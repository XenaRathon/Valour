# Valour, patched for a community node

This branch holds **only** the patches and the build workflow. It does not contain Valour's code.

Every 6 hours, [`build.yml`](.github/workflows/build.yml) takes the latest commit on upstream
[`Valour-Software/Valour`](https://github.com/Valour-Software/Valour) `main`, applies everything in
[`patches/`](patches/), builds the server image and publishes it as:

- `ghcr.io/xenarathon/valour:fasc-latest`, the newest build
- `ghcr.io/xenarathon/valour:fasc-<upstream short sha>`, one tag per upstream commit

So the image tracks upstream exactly like the official `main-latest`, with these changes on top.
If a patch stops applying, the run fails and opens an issue, and nothing new is published.

## Patches

| Patch | Why |
|---|---|
| `0001` privacy route accepts `PlanetManagement` | Community nodes give federated users sessions without `FullControl`, so the owner of a community-hosted planet could never make it public (403). The route still allows only the owner, and the change still needs the owner's device-signed membership log entry. Upstream issue [#1720](https://github.com/Valour-Software/Valour/issues/1720). |

Drop a patch once upstream fixes the problem; when `patches/` is empty, switch back to the official image.

## Updating a patch

```sh
git clone https://github.com/Valour-Software/Valour && cd Valour
git am --3way /path/to/patches/*.patch   # fix conflicts, then git am --continue
git format-patch -o /path/to/patches/ origin/main
```

Valour is licensed under the AGPL-3.0; these patches are offered under the same licence.

*Built with help from Claude (Anthropic). Review before relying on it.*
