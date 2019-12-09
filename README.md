# aws/amazon-firefox-dev as a Zsh package

##### NPM link: [https://www.npmjs.com/package/zsh-firefox-dev](https://www.npmjs.com/package/zsh-firefox-dev)

##### Homepage link: [Mozilla Firefox Developer Edition](https://www.mozilla.org/en-US/firefox/developer/)

| **Package source:** | Source Tarball | Binary | Git | Node | Gem |
|:-------------------:|:--------------:|:------:|:---:|:----:|:---:|
| **Status:**         |  -             | + <br> (default) |  -  |   –  |  –  |

[Zplugin](https://github.com/zdharma/zplugin) can use the NPM package registry
to automatically:

- get the plugin's Git repository OR release-package URL,
- get the list of the recommended ices for the plugin,
    - there can be multiple lists of ices,
    - the ice lists are stored in *profiles*; there's at least one profile, *default*,
    - the ices can be selectively overriden.

Example invocations that'll install [Mozilla Firefox Developer Edition](https://www.mozilla.org/en-US/firefox/developer/)

```zsh
# Download the binary of amazon-firefox-dev command
zplugin pack for firefox-dev

# Download the firefox-dev binary with use of the bin-gem-node annex
zplugin pack"bgn" for firefox-dev
```

## Default Profile

Provides the CLI commands `firefox-bin` and `firefox` by extending the `$PATH`
to point to the snippet's directory.

The Zplugin command executed will be equivalent to:

```zsh
zplugin id-as"firefox-dev" as"command" lucid mv"firefox-dev -> firefox.tar.bz2" \ 
    atclone"rm -rf *~*.tar.bz2(N); tar jxf *.tar.bz2; mv firefox-dev ff; \
        mv ff/*(D) . >/dev/null; rmdir ff" \
    pick"firefox(|-bin)" atpull"%atclone" nocompile is-snippet for \
        "https://download.mozilla.org/?product=firefox-devedition-latest-ssl&os=${${${(M)OSTYPE##linux}:+linux64}:-${${(M)OSTYPE##darwin}:+osx}}&lang=en-US"
```

## bin-gem-node Profile

Provides the CLI command `firefox` by creating a forwarder script (a *shim*) to
the `firefox-bin` command, in `$ZPFX/bin` by using the
[bin-gem-node](https://github.com/zplugin/z-a-bin-gem-node) annex. It's the best
method of providing the binary to the command line.

The Zplugin command executed will be equivalent to:

```zsh
zplugin id-as"firefox-dev" as"null" lucid mv"firefox-dev -> firefox.tar.bz2" \
    atclone"rm -rf *~*.tar.bz2(N); tar jxf *.tar.bz2; mv firefox ff; \
        mv ff/*(D) . >/dev/null; rmdir ff" \
    atpull"%atclone" nocompile is-snippet for \
        "https://download.mozilla.org/?product=firefox-devedition-latest-ssl&os=${${${(M)OSTYPE##linux}:+linux64}:-${${(M)OSTYPE##darwin}:+osx}}&lang=en-US"
```

<!-- vim:set ft=markdown tw=80 fo+=an1 autoindent: -->
