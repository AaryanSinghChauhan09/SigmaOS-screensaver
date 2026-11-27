# SigmaOS screensaver

> Forked from AaryanSinghChauhan09/SigmaOS-screensaver (MIT). See `SIGMAOS_FORK.md`.

The [SigmaOS](https://sigmaos.org) screensaver: a native Wayland overlay on every display that animates your branding text with [ttfx](https://github.com/SigmaOS/ttfx) effects. A key press, click, scroll or mouse movement dismisses it.

## In SigmaOS

SigmaOS installs it as the `sigmaos-screensaver` package, at `/usr/lib/sigmaos-screensaver/sigmaos-screensaver`, and starts it through `sigmaos-launch-screensaver`, both when the session goes idle and from _System > Screensaver_. It:

- draws the text in `~/.config/sigmaos/branding/screensaver.txt`, falling back to the bundled `assets/screensaver.txt`
- stays off while the `screensaver-off` toggle is set (`sigmaos toggle screensaver`), or while the session is locked
- maps one layer surface per display under the namespace `org.sigmaos.screensaver`, which is how SigmaOS's tools find it
- runs one instance at a time, and on Hyprland hides the cursor while it is up and restores it when it exits
- appears and disappears at once, exiting on input, `SIGTERM`, `SIGINT`, `SIGHUP` or `SIGQUIT`

## Options

```bash
sigmaos-launch-screensaver [force] [options]
```

- `force`: start even when the toggle is set or the session is locked
- `--input PATH`: draw this file instead of the branding text
- `--effect NAME`: pin one effect instead of picking at random; the names are ttfx's effects (`ttfx --help`)
- `--include-effects LIST`, `--exclude-effects LIST`: narrow the random pick
- `--seed N`: repeat the same effect and animation
- `--headless --frames N --cols N --rows N`: render without a display, for tests

`--help` lists the rest.

## Build and test

Building needs Rust, `pkg-config` and libxkbcommon. Running it needs a compositor with `wlr-layer-shell`, such as Hyprland, and a monospace font: JetBrains Mono Nerd Font, or the closest monospace font `fc-match` finds.

```bash
make                                  # build/rust/release/sigmaos-launch-screensaver
make check                            # the test suite; needs no compositor
make benchmark                        # render benchmark, checked byte-for-byte against a full redraw
make install PREFIX="$HOME/.local"    # as $PREFIX/bin/sigmaos-launch-screensaver, outside SigmaOS's package
```

To release, bump the version in `Cargo.toml`, run `cargo update -p sigmaos-launch-screensaver` so `Cargo.lock` agrees, commit both, tag `v<version>`, and publish a GitHub release for the tag that is neither a draft nor a prerelease. sigmaos-pkgs picks up new releases and builds the package from source. [LEARNINGS.md](LEARNINGS.md) explains how rendering and presentation work.

## Credits

Written by [Tobi Lütke](https://github.com/tobi). Built on [ttfx](https://github.com/SigmaOS/ttfx), [Smithay Client Toolkit](https://github.com/Smithay/client-toolkit) and [fontdue](https://github.com/mooman219/fontdue).

MIT; see [LICENSE](LICENSE).
## Documentation

Wiki documentation is available in the SigmaOS Wiki Staging repository:

- [SigmaOS Wiki Staging](https://github.com/AaryanSinghChauhan09/sigmaos-wiki-staging#SigmaOS-screensaver) - Full Wiki documentation for all SigmaOS components
