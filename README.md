# darwish1812.github.io

The root of this GitHub Pages account. It exists for one file.

## Why this repo has to exist

Android verifies that an app may claim a website by fetching

```
https://darwish1812.github.io/.well-known/assetlinks.json
```

from the **origin root** — not from the folder the site happens to live in. Marginalia is
published at `/marginalia/accounts3/`, and a file there would never be read.

On GitHub Pages the origin root is served by a repository named exactly
`<account>.github.io`. That is the whole reason for this repository.

Until the file answered, Chrome refused the claim and said so:

```
digital_asset_links_handler.cc(135) Digital Asset Links endpoint responded with code 404.
```

and the app opened as a Custom Tab with the address bar showing across the top, looking
like a browser somebody had forgotten to hide.

## `.nojekyll` is not optional

GitHub Pages runs Jekyll by default, and **Jekyll skips every file and folder whose name
begins with a dot**. `.well-known` begins with a dot. Without the empty `.nojekyll` marker
at the root, the one file this repository exists to serve is the one file that never gets
published — and nothing says so: the site builds, reports success, and returns 404 for the
only path that matters.

## What is in the file

One fingerprint: the SHA-256 of the certificate that signs the app. It is public by
design — it proves which key signed the APK, and it carries no secret.

`sha256_cert_fingerprints` is an **array**, and it will need a second entry if this app ever
reaches Google Play. Play re-signs every upload with its own key, so the fingerprint that
matters there is Google's app-signing certificate from the Play Console, not the local one.
Listing both lets a sideloaded build and a store build verify against the same file.
Listing only the upload key is the most common reason a published TWA still shows a URL bar.

## What this claims

Verification is per **origin**, so this file lets the app claim the whole of
`darwish1812.github.io`, not just the booklet. That is acceptable for a demo. For anything
public, give the app a domain of its own so its reach is only what it needs.

## Checking it

```
curl -s https://darwish1812.github.io/.well-known/assetlinks.json
```

Google's own validator:

```
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=https://darwish1812.github.io&relation=delegate_permission/common.handle_all_urls
```

Then reinstall the app. Verification is cached, so an app installed while the file was
missing keeps its address bar until it is installed again.
