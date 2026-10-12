# libfprint-goodixtls511-git (VibeProgramm fork build)

AUR-style packaging of `libfprint` with the Goodix GF511 (`27c6:5110`) TLS
driver, built from `VibeProgramm/libfprint` branch
`fix/read-loop-error-spin` (read-loop resubmit-spin fix on top of
`buxel/libfprint` `goodixtls-on-1.94.9`).

Install locally:

```sh
makepkg -s
sudo pacman -U libfprint-goodixtls511-git-*.pkg.tar.zst
```

## Auto-sync

`pkgver` for `-git` packages is dynamic (`pkgver()` counts the fork branch),
but AUR also needs a static snapshot in `.SRCINFO`.
The `sync-pkgver` workflow keeps both fresh:

* on every push to `main` touching `PKGBUILD`/workflow,
* daily via cron (polls the fork branch for new commits),
* manually via `workflow_dispatch`.

It clones the fork branch into `src/`, recomputes `pkgver`, regenerates
`.SRCINFO` and commits the result back to `main`. So a push to the
libfprint fork is picked up automatically — at most a day later, or
immediately via manual dispatch.

## Optional: auto-publish to AUR

Disabled by default. To enable:

1. You must own (or have co-maintainership of) the AUR package name.
   `PKGBUILD` here uses `libfprint-goodixtls511-git`; if that name is taken,
   set repo variable `AUR_PKG` to your own package base instead.
2. Add an AUR SSH key: repo secret `AUR_SSH_KEY` (private key registered
   at https://aur.archlinux.org/account).
3. Set repo variable `AUR_PUSH=true`.

Then the `aur-publish` job pushes `PKGBUILD`/`.SRCINFO` to AUR after
every successful sync.
